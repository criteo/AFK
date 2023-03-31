# Installation

## Existing NetBox instance

This is simple:

1. Download the `network_cmdb` plugin from the [criteo/netbox-network-cmdb](https://github.com/criteo/netbox-network-cmdb) repository.
2. Place the directory in your NetBox instance.
3. Open the `netbox/configuration.py` file and add `netbox_cmdb` in the `PLUGINS` list.
4. Restart your NetBox instance.

## From scratch

You can have a look at the `docker-compose.yml` and the other scripts in the [develop directory](https://github.com/criteo/netbox-network-cmdb/tree/main/develop).

* For development/test, simply run `make start` from the [Network CMDB](https://github.com/criteo/netbox-network-cmdb) repository.
* For production, please ensure to integrate it properly to your environment.

## Access the UI

For now, there are no CMDB components in NetBox UI. It will be added later.

In the meantime you can access the CMDB in the Django Admin UI: http://127.0.0.1:8000/admin/
