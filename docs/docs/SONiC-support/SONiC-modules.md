# SONiC modules

## How to install

The installation process is similar to [SaltStack modules installation](/SaltStack-modules/installation/).

1. Download the [codebase](https://github.com/criteo/sonic-saltstack) to a dedicated path on your Salt-master.
2. Add a new path to the `file_roots` section in the Salt-master configuration.

Example of salt-master configuration:

```yaml
file_roots:
  base:
    # ... other paths ...
    # SONiC codebase:
    - /srv/salt/base/sonic/
```

!!! warning "Important"

    Make sure to synchronize the modules with your minions:

    ```
    salt <device> saltutil.sync_all
    ```
