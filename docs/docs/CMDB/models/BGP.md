# BGP

## Source code

Models location: [netbox_cmdb/models/bgp.py](https://github.com/criteo/netbox-network-cmdb/blob/main/netbox_cmdb/netbox_cmdb/models/bgp.py)

## Table relations

!!! info

    To simplify the diagram, not all relations appear (example: `DeviceBGPSession` => `IPAM.IPAddress`)

``` mermaid
erDiagram

    dcim_Device 1--1 BGPGlobal: ""
    dcim_Device 1--0+ BGPSession: ""
    BGPSession 1--1+ DeviceBGPSession: ""
    DeviceBGPSession 1--0+ AfiSafi: ""
    DeviceBGPSession 1--0+ RoutePolicy: ""
    AfiSafi 1--0+ RoutePolicy: ""
```

## BGP global configuration

```yaml
BGPGlobal:
    device: dcim.Device
    local_asn: cmdb.ASN
    router_id: string
    ebgp_administrative_distance: integer
    ibgp_administrative_distance: integer
    graceful_restart: boolean
    graceful_restart_time: integer
    ecmp: boolean
    ecmp_maximum_paths: integer
```

```yaml
ASN:
    organization_name: string
    number: integer
```

## BGP sessions

The implementation for BGP sessions is more complex.

The main idea is to have a `BGPSession` linked to two `DeviceBGPSession` to avoid data duplication such as `local-asn` vs `remote-asn`.

!!! info

    As we have deprecated the usage of peer-groups, we do not document the model here.

```yaml
BGPSession:
    status: boolean
    peer_a: cmdb.DeviceBGPSession
    peer_b: cmdb.DeviceBGPSession
    password: string
    circuit: cmdb.Circuit
    tenant: dcim.Tenant
```

```yaml
DeviceBGPSession:
    device: dcim.Device
    description: string
    local_address: netbox.ipam.IPAddress
    remote_asn: cmdb.ASN
    peer_group: cmdb.PeerGroup (deprecated)
    maximum_prefixes: integer
    route_policy_in: cmdb.RoutePolicy
    route_policy_out: cmdb.RoutePolicy
    enforce_first_as: boolean (not used yet)
```

```yaml
AfiSafi:
    device: dcim.Device
    route_policy_in: cmdb.RoutePolicy
    route_policy_out: cmdb.RoutePolicy
    device_bgp_session: cmdb.DeviceBGPSession
```
