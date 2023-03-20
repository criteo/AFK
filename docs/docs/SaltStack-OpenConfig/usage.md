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

* Dry run: `salt <device> state.apply full_config test=True`
* Deploy: `salt <device> state.apply full_config`

## Schedule configuration deployment

!!! info

    These are examples, don't forget to adapt them to your infrastructure

Simple schedule in `{SALT_PILLAR_PATH}/schedule_simple_full_config.sls`

```yaml
schedule:
  full_config:
    function: state.sls
    args:
      - full_config
    minutes: 30
    range:
      start: 8am
      end: 7pm
```

Schedule only for devices having `salt-enabled` tag in NetBox in `{SALT_PILLAR_PATH}/schedule_smart_full_config.sls`

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
    """Get OpenConfig salt-enabled data for all devices."""
    device = __grains__["id"]
    endpoint = f"{DATA_API}/devices/{device}/salt_enabled"

    try:
        result = requests.get(endpoint, auth=(USER, PASSWORD)).json()
        if result.get("salt_enabled") is True:
            return {
                "schedule": {
                    "full_config": {
                        "function": "state.sls",
                        "args": ["full_config"],
                        "minutes": 30,
                        "range": {"start": "8am", "end": "7pm"},
                    }
                }
            }
    except Exception as error:
        logging.error("data-aggregation-api: failed to query '%s' because %s", endpoint, error)

    return {}
```

!!! danger

    If you are putting secrets directly in the pillar file, don't forget to apply the appropriate permissions to the file. Something like `chmod 600`.