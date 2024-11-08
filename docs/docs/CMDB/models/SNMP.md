# SNMP

## Source code

Models location: [netbox_cmdb/models/snmp.py](https://github.com/criteo/netbox-network-cmdb/blob/main/netbox_cmdb/netbox_cmdb/models/snmp.py)

## Table relations

!!! info

    To simplify the diagram, not all relations are displayed (example: `DeviceBGPSession` => `IPAM.IPAddress`)

``` mermaid
erDiagram

    dcim_Device 1--1+ SNMP: ""
    SNMP 1+--1+ SNMPCommunity: ""
```

## BGP global configuration

```yaml
SNMP:
    device: dcim.Device
    contact: string
    location: string
    community_list: cmdb.SNMPCommunity
```

```yaml
SNMPCommunity:
    SNMPCommunity: string
    SNMPCommunity: string
    type: RO|RW
```
