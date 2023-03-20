# SONiC modules

## How to install

The installation process is similar to [SaltStack OpenConfig installation](/SaltStack-OpenConfig/installation.md).

You need to download the [codebase](https://github.com/criteo/sonic-saltstack) to a dedicated path on your Salt-master.

Then you must tell the Salt-master that there is a new path to watch.

Example of salt-master configuration:

```yaml
file_roots:
  base:
    # ... other paths ...
    # SONiC codebase:
    - /srv/salt/base/sonic/
```

Do not forget to synchronize the modules with your minions:

    salt <device> saltutil.sync_all

