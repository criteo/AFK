# Installation

| Component | How to install |
|-----------|----------------|
| NetBox                  | [https://docs.netbox.dev/en/stable/installation/](https://docs.netbox.dev/en/stable/installation/)
| [Network CMDB](https://github.com/criteo/netbox-network-cmdb)             | Network CMDB plugin for Netbox                            | ![Last commit](https://img.shields.io/github/last-commit/criteo/netbox-network-cmdb/main) |
| Data Aggregation API    | see our [Installation guide](Data-Aggregation-API/installation.md)
| SaltStack               | [https://docs.saltproject.io/salt/install-guide/en/latest/](https://docs.saltproject.io/salt/install-guide/en/latest/)
| SaltStack for JunOS/EOS | [proxy-minion](https://docs.saltproject.io/en/latest/topics/proxyminion/index.html) with [napalm driver](https://github.com/napalm-automation/napalm-salt)
| SaltStack for SONiC     | see our [SONiC Salt Deployer](/SONiC-support/overview) and our [AFK modules for SaltStack](/SaltStack-modules/installation/)

!!! note

    If you are not familiar with NetBox and SaltStack, you should look at their awesome documentation prior to trying AFK:

    - [https://docs.netbox.dev/](https://docs.netbox.dev/)
    - [https://docs.saltproject.io/salt/user-guide/](https://docs.saltproject.io/salt/user-guide/)

!!! info

    We plan to provide a script to deploy a development environment. You will be able to inspire yourself from this to deploy to production.