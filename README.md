# Criteo AFK - Automation Framework for networK

## Context

At Criteo, we have decided to fully open source our network automation stack.

It is based on [NetBox](https://netbox.dev), [OpenConfig](https://www.openconfig.net/), [SaltStack](https://github.com/SaltStack/salt), and supports Juniper JunOS, Arista EOS and [SONiC](https://sonic-net.github.io/SONiC/).

The full documentation can be found here: [https://criteo.github.io/AFK/](https://criteo.github.io/AFK/)

> **Warning**
>
> **If you are using an ad-blocker, the documentation website might not work properly because `Criteo` is in the URL.**

## Repositories

| Repository | Description | Latest commit |
|------------|-------------|---------------|
| [Network CMDB](https://github.com/criteo/netbox-network-cmdb)             | Network CMDB plugin for Netbox                            | ![Last commit](https://img.shields.io/github/last-commit/criteo/netbox-network-cmdb/main) |
| [Data aggregation API](https://github.com/criteo/data-aggregation-api)    | Aggregate data from CMDB and convert to OpenConfig        | ![Last commit](https://img.shields.io/github/last-commit/criteo/data-aggregation-api/main) |
| [AFK Salt modules](https://github.com/criteo/afk-saltstack)               | Salt modules to apply configuration from OpenConfig data  | ![Last commit](https://img.shields.io/github/last-commit/criteo/openconfig-SaltStack/main) |
| [SONiC Salt Deployer](https://github.com/criteo/sonic-salt-deployer)      | tool to deploy and configure salt-minion on SONiC devices | ![Last commit](https://img.shields.io/github/last-commit/criteo/sonic-salt-deployer/main) |
| [SONiC SaltStack](https://github.com/criteo/sonic-saltstack)              | Salt States/Execution modules for SONiC                   | ![Last commit](https://img.shields.io/github/last-commit/criteo/sonic-SaltStack/main) |
| [SONiC utilities](https://github.com/criteo/criteo-sonic-utilities)       | SONiC scripts used by some SONiC SaltStack modules        | ![Last commit](https://img.shields.io/github/last-commit/criteo/criteo-sonic-utilities/main) |
