# Overview

## Supported versions

| SONiC version | support |
|---------------|---------|
| 201911        | :white_check_mark: |
| 202205        | :white_check_mark: |

!!! info "Legend"

    - :white_check_mark: supported
    - :material-progress-wrench: work in progress
    - :warning: deprecated
    - :no_entry: not supported

## Installation

| Step | Description | Guide |
|------|-------------|-------|
| 1    | install `salt-minion` on your SONiC devices | [SONiC Salt Deployer](SONiC-Salt-Deployer.md) section |
| 2    | deploy `Criteo SONiC utilities`             | [Criteo SONiC utilities](Criteo-SONiC-utilities.md) section |
| 3    | deploy our SONiC modules                    | [SONiC modules](SONiC-modules.md) |
| 4    | deploy our OpenConfig modules               | [SaltStack-OpenConfig](/SaltStack-OpenConfig/installation) section |

!!! warning "Important"

    To benefit from all AFK features, you need to change FRR integration in SONiC.

    By default, the files in `/etc/frr` of the BGP container are generated from an embedded template combined with metadata from the `config_db`.

    AFK requires to directly mount `/etc/frr` of the BGP container to `/etc/sonic/frr` on the host.

    The change has been upstream starting 202205. To configure SONiC this way:

    * for SONiC < 202205, you need to apply our [patch manually](https://github.com/criteo/criteo-sonic-utilities#frr-mounted-configuration)
    * for SONiC >= 202205, you need to enable [split-unified](https://github.com/sonic-net/sonic-buildimage/commit/9d3814045bf950576bb274180ffec001abac1c32)
