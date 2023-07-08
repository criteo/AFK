# Usage

## Setup OpenConfig pillar

To apply the configuration, you need the input data in OpenConfig format (JSON RFC7951).

If you are using our [Data Aggregation API](/Data-Aggregation-API), you can create the following pillar in `{SALT_PILLAR_PATH}/data-aggregation-api-openconfig.sls`:

```py
#!py
import logging
import requests

DATACENTER = "paris"
ENVIRONMENT = "production"
USER = "salt-master"
PASSWORD = "awesomepassword"

DATA_API = f"https://data-aggregation-api.{DATACENTER}.{ENVIRONMENT}.local"


def run():
    """Get OpenConfig data for all devices."""
    device = __grains__["id"]
    openconfig_endpoint = f"{DATA_API}/devices/{device}/openconfig"

    try:
        result = requests.get(openconfig_endpoint, auth=(USER, PASSWORD)).json()
        return {"openconfig": result}
    except Exception as error:
        logging.error("data-aggregation-api: failed to query '%s' because %s", openconfig_endpoint, error)
        return {}
```

## Apply the configuration manually

* Dry run: `salt <device> state.apply afk test=True`
* Deploy: `salt <device> state.apply afk`

## Schedule configuration deployment

!!! info

    These are examples. Make sure to adapt them to your infrastructure.

Simple schedule in `{SALT_PILLAR_PATH}/schedule_simple_afk.sls`

```yaml
schedule:
  afk:
    function: state.sls
    args:
      - afk
    minutes: 30
    range:
      start: 8am
      end: 7pm
```

Schedule only for devices having a `afk-enabled` tag in NetBox in `{SALT_PILLAR_PATH}/schedule_smart_afk.sls`

```py
#!py
import logging
import requests

DATACENTER = "paris"
ENVIRONMENT = "production"
USER = "salt-master"
PASSWORD = "awesomepassword"

DATA_API = f"https://data-aggregation-api.{DATACENTER}.{ENVIRONMENT}.local"


def run():
    """Get AFK afk-enabled data for all devices."""
    device = __grains__["id"]
    endpoint = f"{DATA_API}/devices/{device}/salt_enabled"

    try:
        result = requests.get(endpoint, auth=(USER, PASSWORD)).json()
        if result.get("salt_enabled") is True:
            return {
                "schedule": {
                    "afk": {
                        "function": "state.sls",
                        "args": ["afk"],
                        "minutes": 30,
                        "range": {"start": "8am", "end": "7pm"},
                    }
                }
            }
    except Exception as error:
        logging.error("data-aggregation-api: failed to query '%s' because %s", endpoint, error)

    return {}
```

!!! danger "Attention"

    If you are putting secrets directly in the pillar file, make sure to apply the appropriate permissions to the file. Something like `chmod 600`.