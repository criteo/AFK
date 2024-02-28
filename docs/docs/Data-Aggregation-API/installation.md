# Installation

## Quickstart

1. Download the latest release archive.
2. Extract the single binary.
3. Configure the Data Aggregation API accordingly.
4. Run the binary.

Example of basic configuration (`settings.yml`):
```yaml
Datacenter: "europe"
Log:
  Level: "error"
  Pretty: true

API:
  ListenAddress: "127.0.0.1"
  ListenPort: 1234

Log:
  Level: "info"
  Pretty: true

NetBox:
  URL: "https://netbox.local"
  APIKey: "<some_key>"
  DatacenterFilterKey: "site"

Build:
  Interval: "30m"
```

See [Configuration](./configuration.md) for more details.


## Dependencies

The only dependency is our [Network CMDB](https://github.com/criteo/netbox-network-cmdb) NetBox plugin.

The installation guide of this plugin is available in the CMDB section [here](/docs/docs/CMDB/).
