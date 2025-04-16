# Features

## Legend

* :white_check_mark: supported
* :material-progress-wrench: work in progress
* :octicons-project-roadmap-16: in the roadmap
* :no_entry: will probably not be implemented

## BGP

| Features | SONiC | JunOS | EOS |
|----------|-------|-------|-----|
| Global configuration                  | :white_check_mark: | :white_check_mark: | :white_check_mark: |
| Peers: IPv4 unicast and IPv6 unicast  | :white_check_mark: | :white_check_mark: | :white_check_mark: |
| Peers: EVPN                           | :octicons-project-roadmap-16: | :octicons-project-roadmap-16: | :octicons-project-roadmap-16: |
| Peer Groups (deprecated)              | :white_check_mark: | :white_check_mark: | :white_check_mark: |

## Routing Policy

| Features | SONiC | JunOS | EOS |
|----------|-------|-------|-----|
| Route maps           | :white_check_mark: | :white_check_mark: | :white_check_mark: |
| Community lists      | :white_check_mark: | :white_check_mark: | :white_check_mark: |
| Prefix lists         | :white_check_mark: | :white_check_mark: | :white_check_mark: |
| Extended communities | :octicons-project-roadmap-16: | :octicons-project-roadmap-16: | :octicons-project-roadmap-16: |
| Large communities    | :octicons-project-roadmap-16: | :octicons-project-roadmap-16: | :octicons-project-roadmap-16: |

## Interfaces

| Features | SONiC | JunOS | EOS |
|----------|-------|-------|-----|
| Physical interface | :material-progress-wrench: | :material-progress-wrench: | :material-progress-wrench: |
| Logical interface  | :material-progress-wrench: | :material-progress-wrench: | :material-progress-wrench: |

## Routing and switching

| Features | SONiC | JunOS | EOS |
|----------|-------|-------|-----|
| VRF      | :material-progress-wrench: | :material-progress-wrench: | :material-progress-wrench: |
| VLAN     | :material-progress-wrench: | :material-progress-wrench: | :material-progress-wrench: |

## System


| Features | SONiC | JunOS | EOS |
|----------|-------|-------|-----|
| SNMP     | :white_check_mark: | :white_check_mark: | :white_check_mark: |
| Syslog   | :octicons-project-roadmap-16: | :octicons-project-roadmap-16: | :octicons-project-roadmap-16: |
| TACACS   | :octicons-project-roadmap-16: | :octicons-project-roadmap-16: | :octicons-project-roadmap-16: |
| Users    | :octicons-project-roadmap-16: | :octicons-project-roadmap-16: | :octicons-project-roadmap-16: |
| SSH      | :no_entry: | :no_entry: | :no_entry: |

!!! info

    SSH support will probably not be added because we consider it to be part of a minimal configuration which must be configured on the device during bootstrap (via ZTP or manual).
