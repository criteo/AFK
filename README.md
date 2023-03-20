# Criteo AFK - Automation Framework for networK

## Context

At Criteo, we have decided to fully open source our network automation stack.

It is based on [NetBox](https://netbox.dev), [OpenConfig](https://www.openconfig.net/), [SaltStack](https://github.com/SaltStack/salt), and supports Juniper JunOS, Arista EOS and [SONiC](https://sonic-net.github.io/SONiC/).

## Repositories

| Repository | Description | Latest commit |
|------------|-------------|---------------|
| [Network CMDB](https://github.com/criteo/netbox-network-cmdb)             | Network CMDB plugin for Netbox                            | ![Last commit](https://img.shields.io/github/last-commit/criteo/netbox-network-cmdb/main) |
| Data aggregation API (coming soon)                                        | Aggregate data from CMDB and convert to OpenConfig        | |
| [OpenConfig Salt modules](https://github.com/criteo/openconfig-SaltStack) | Salt modules to apply configuration from OpenConfig data  | ![Last commit](https://img.shields.io/github/last-commit/criteo/openconfig-SaltStack/main) |
| [SONiC Salt Deployer](https://github.com/criteo/sonic-salt-deployer)      | tool to deploy and configure salt-minion on SONiC devices | ![Last commit](https://img.shields.io/github/last-commit/criteo/sonic-salt-deployer/main) |
| [SONiC SaltStack](https://github.com/criteo/sonic-SaltStack)              | states/execution modules for SONiC                        | ![Last commit](https://img.shields.io/github/last-commit/criteo/sonic-SaltStack/main) |
| [SONiC utilities](https://github.com/criteo/criteo-sonic-utilities)       | SONiC scripts used by some SONiC SaltStack modules        | ![Last commit](https://img.shields.io/github/last-commit/criteo/criteo-sonic-utilities/main) |

## Global design

Our approach to automation is opinionated. There are tons of ways of doing network configuration, and choices must be made.

                       ┌──────────────┐     ┌────────────────────┐
                       │ Network CMDB │     │ Other data sources │
                       └──────────┬───┘     └──┬─────────────────┘
                                  │            │
                                  │            │
                                  │            │
                                  │            │
                               ┌──▼────────────▼──┐
                               │ Data aggregation │
                               │        API       │
                               └─────────┬────────┘
                                         │
                                         │
                                         │
                                   ┌─────▼─────┐
                                   │ SaltStack │
                                   └─────┬─────┘
                                         │
                                         │
                                         │
                                         │
                                ┌────────▼────────┐
                                │ Network devices │
                                └─────────────────┘

### Network CMDB

The Network CMDB contains data relative to the business and is completely agnostic to the network OS.

The models are designed to describe the objects themselves rather than the configuration from device perspective. The idea is also to avoid any data duplication which could lead to configuration mismatchs.

For instance, we represent the BGP session itself with two joined table describing peers: `DeviceBGPSession` <==> `BGPSession` <==> `DeviceBGPSession`.

* `DeviceBGPSession` contains the `local-as` but not the `peer-as`, avoiding data duplication. The `peer-as` being the `local-as` of the other neighbor.
* `BGPSession` contains all information peers have in common, like status (`in production`, `maintenance` etc...) or `MD5 password`

### Data Aggregation API

This API aggregates data from their source of truth: Network CMDB or possibly any other data sources.

Then, it computes these data to provide OpenConfig JSON for each device as an output.

[ygot](https://github.com/openconfig/ygot) is used to validate the output against the OpenConfig YANG models.

### SaltStack OpenConfig

Salt will take OpenConfig data and convert them as Network configuration. We are using templates to do that.

The end goal is to simply forward this OpenConfig data to the Network OS so it can apply it. Currently, OpenConfig is, at best, partially implemented in Network Operating Systems.
