# Configuration

## Configuration file

The Data Aggregation API can be configured with flags, environments variables and configuration file.

The precedence order for the different methods is:

!!! info

    * flags
    * environment variables
    * configuration file (settings.yml)

```yaml
Datacenter: "europe"

Log:
  Level: "error"
  Pretty: true

API:
  ListenAddress: "127.0.0.1"
  ListenPort: 1234

Authentication:
  LDAP:
    URL: "ldaps://URL.local"
    BindDN: "cn=<user>,OU=<ou>,DC=<local>"
    BaseDN: "DC=<local>"
    Password: "<some_password>"
    WorkersCount: 10
    Timeout: 5s
    MaxConnectionLifetime: 1m
    InsecureSkipVerify: false

NetBox:
  URL: "https://netbox.local"
  APIKey: "<some_key>"
  DatacenterFilterKey: "site"
  LimitPerPage: 500

Build:
  Interval: "30m"
  AllDevicesMustBuild: false
```

## Global settings

| Parameter | Default | Description |
|-----------|---------|-------------|
| Datacenter | `` | Value used to filter devices. The key is defined by `DatacenterFilterKey`. |

## Log settings

All parameters below are in the `Log` section of the configuration.
This section is optional.

| Parameter | Default | Description |
|-----------|---------|-------------|
| Level | `info` | Log level. |
| Pretty | `false` | If enabled: human readable logs (with colors). If disabled: structured logs. |

## API settings

All parameters below are in the `API` section of the configuration.
This section is optional.

| Parameter | Default | Description |
|-----------|---------|-------------|
| ListenAddress | `0.0.0.0` | Listening address of the web API. |
| ListenPort | `8080` | Listening port of the web API. |

## Authentication settings

All parameters below are in the `Authentication`->`LDAP` section of the configuration.

LDAP Authentication is only to authenticate users when they try to query the Web API, i.e. when they want to retrieve the built config.

This section is optional.

| Parameter | Default | Description |
|-----------|---------|-------------|
| InsecureSkipVerify | `false` | Ignore LDAP TLS warnings. |
| URL | | URL of the LDAP server. |
| BindDN |  | Bind used to query the LDAP server. |
| Password |  | Password to query the LDAP server. |
| BaseDN |  | Only users matching the BaseDN are authorized to query the web API. |
| WorkersCount | `10` | Number of workers to authenticate users concurrently. |
| Timeout | `10s` | Time to wait before considering a LDAP request timed out. |
| MaxConnectionLifetime | `1m` | Lifetime of worker connection to LDAP. Useful to re-use existing LDAP connection. |


## NetBox settings

All parameters below are in the `NetBox` section of the configuration.

| Parameter | Default | Description |
|-----------|---------|-------------|
| URL | | NetBox base URL. |
| APIKey | | NetBox API token. |
| DatacenterFilterKey | `site` | Key used to filter devices, using `Datacenter` as a value. |
| LimitPerPage | `100` | Number of elements per page when getting data from NetBox API. |

## Build settings

All parameters below are in the `NetBox` section of the configuration.
This section is optional.

| Parameter | Default | Description |
|-----------|---------|-------------|
| Interval | `1m` | Build interval, e.g.: `10m`, `1h` |
| AllDevicesMustBuild | `false` | The build fails if one device has not been built. |

## Alternative methods

### Environment variables

All settings available in the configuration file can be set as environment variables, but:

* all variables must be prefixed by `DAAPI_`
* uppercase only
* `_` is the level separator

For example, the equivalent of this config file:

``` yaml
Datacenter: "europe"
NetBox:
  URL: "https://netbox.local"
  APIKey: "<some_key>"
```

is:

```shell
DAAPI_DATACENTER="europe"
DAAPI_NETBOX_URL="https://netbox.local"
DAAPI_NETBOX_APIKEY="<some_key>"
```
