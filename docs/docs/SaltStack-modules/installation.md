# Installation

!!! warning "Be careful"

    * These modules are under active development and subject to changes.
    * The implementation is opiniated and might not be appropriate for your environment.
    * You should always test new releases on your infrastructure before going to production.

## How to install

1. Download the [codebase](https://github.com/criteo/openconfig-saltstack) to a dedicated path on your Salt-master.
2. Add a new path to the `file_roots` section in the Salt-master configuration.

Example of salt-master configuration:
```yaml
file_roots:
  base:
    # your own codebase:
    - /srv/salt/base/your-code-base/
    # OpenConfig codebase:
    - /srv/salt/base/openconfig/
```

!!! warning "Important"

    Make sure to synchronize the modules with your minions:

    ```
    salt <device> saltutil.sync_all
    ```

## Dependencies

Depending on the Network OS you want to support, you will need:

* [SONiC modules](/SONiC-support/overview)
* [napalm-salt](https://github.com/napalm-automation/napalm-salt) for Juniper JunOS and Arista EOS
