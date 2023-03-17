# Criteo AFK - Automation Framework for networK

## Context

At Criteo, we have decided to fully opensource our automation stack. It is based on Saltstack, OpenConfig and supports Juniper JunOS, Arista EOS and SONiC.

This repository aims to help to document and navigate our stack, as it consists of different repositories.
It will include our documentation, our FAQ, and potentially other things.

At the moment, not everything is opensource as we are releasing it progressively (some cleaning is needed on our side).

## Repositories

| Repository | Description | Latest commit |
|------------|-------------|---------------|
| [Network CMDB](https://github.com/criteo/netbox-network-cmdb)             | Network CMDB plugin for Netbox                            | ![Last commit](https://img.shields.io/github/last-commit/criteo/netbox-network-cmdb/main) |
| Data aggregation API (coming soon)                                        | Aggregate data from CMDB and convert to OpenConfig        | |
| [OpenConfig Salt modules](https://github.com/criteo/openconfig-saltstack) | Salt modules to apply configuration from OpenConfig data  | ![Last commit](https://img.shields.io/github/last-commit/criteo/openconfig-saltstack/main) |
| [SONiC Salt Deployer](https://github.com/criteo/sonic-salt-deployer)      | tool to deploy and configure salt-minion on SONiC devices | ![Last commit](https://img.shields.io/github/last-commit/criteo/sonic-salt-deployer/main) |
| [SONiC Saltstack](https://github.com/criteo/sonic-saltstack)              | states/execution modules for SONiC                        | ![Last commit](https://img.shields.io/github/last-commit/criteo/sonic-saltstack/main) |
| [SONiC utilities](https://github.com/criteo/criteo-sonic-utilities)       | SONiC scripts used by some SONiC saltstack modules        | ![Last commit](https://img.shields.io/github/last-commit/criteo/criteo-sonic-utilities/main) |

## Global design

Our approach to automation is opinionated. There are tons of ways of doing network configuration, and choices have to be made at some points.

           ┌──────────────┐     ┌────────────────────┐
           │ Network CMDB │     │ Other data sources │
           └──────────┬───┘     └──┬─────────────────┘
                      │            │
                      │            │
                      │            │
                      │            │
                   ┌──▼────────────▼──┐
                   │ Data aggregation │
                   │  converter API   │
                   └─────────┬────────┘
                             │
                             │
                             │
                       ┌─────▼─────┐
                       │ Saltstack │
                       └─────┬─────┘
                             │
                             │
                             │
                             │
                    ┌────────▼────────┐
                    │ Network devices │
                    └─────────────────┘

### Network CMDB

The Network CMDB contains data relative to the business and is completely agnostic to the network OS. The models are designed to describe the objects themselves rather than the configuration from devices perspective. The main principle is also to avoid any data duplication which could lead to mismatch configuration.

For instance, we represent the BGP session itself with two joined table describing peers: DeviceBGPSession <==> BGPSession <==> DeviceBGPSession.
* DeviceBGPSession contains the local-as but not the peer-as, avoiding data duplication. The peer-as being the local-as of the other neighbor.
* BGPSession contains all information peers have in common, like status (in production, maintenance etc...) or MD5 password

### Data Aggregation API

This API aggregate data from their source of truths (Network CMDB or possibly any other data sources).

Then, it computes these data to provide OpenConfig JSON for each device as an output.

We use OpenConfigValidator to validate data against the OpenConfig YANG models.

### Saltstack

Salt will take OpenConfig data and convert them as Network configuration. At the moment, we are using templates to do that.

The end goal is to simply forward this OpenConfig data to the Network OS so it can apply it. At best, OpenConfig is partially implemented at all in Network Operating Systems.
