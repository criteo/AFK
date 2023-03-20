# Installation

## Existing NetBox instance

This is simple:

* download the `network_cmdb` plugin from [criteo/netbox-network-cmdb](https://github.com/criteo/netbox-network-cmdb) repository
* place the directory in your NetBox instance
* open the `netbox/configuration.py` file and add `netbox_cmdb` in the `PLUGINS` list
* restart your NetBox instance

## From scratch

You can have a look at the `docker-compose.yml` and the other scripts in the [develop directory](https://github.com/criteo/netbox-network-cmdb/tree/main/develop).

* for development/test: simply run `make start` from [Network CMDB](https://github.com/criteo/netbox-network-cmdb) repository
* for production, please ensure to integrate it properly to your environment
