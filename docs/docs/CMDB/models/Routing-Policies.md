# Routing Policies

## Source code

Models locations:

* [netbox_cmdb/models/prefix_list.py](https://github.com/criteo/netbox-network-cmdb/blob/main/netbox_cmdb/netbox_cmdb/models/prefix_list.py)
* [netbox_cmdb/models/bgp_community_list.py](https://github.com/criteo/netbox-network-cmdb/blob/main/netbox_cmdb/netbox_cmdb/models/bgp_community_list.py)
* [netbox_cmdb/models/route_policy.py](https://github.com/criteo/netbox-network-cmdb/blob/main/netbox_cmdb/netbox_cmdb/models/route_policy.py)

## Table relations

``` mermaid
erDiagram

    dcim_Device 1--0+ RoutePolicy: ""
    RoutePolicy 1--0+ RoutePolicyTerm: ""
    RoutePolicyTerm 0+--o| BgpCommunityList: ""
    RoutePolicyTerm 0+--o| PrefixList: ""
    BgpCommunityList 1--0+ BgpCommunityListTerm: ""
    PrefixList 1--0+ PrefixListTerm: ""

```

## Prefix lists

```yaml
PrefixList:
    name: string
    device: dcim.Device
    ip_version: string choices
```

```yaml
PrefixListTerm:
    prefix_list: cmdb.PrefixList
    sequence: integer
    decision: string choices
    prefix: IPNetwork
    le: integer
    ge: integer
```

## Community lists

##

```yaml
BGPCommunityList:
    name: string
    device: dcim.Device
```

```yaml
BGPCommunityListTerm:
    bgp_community_list: cmdb.BGPCommunityList
    sequence: integer
    decision: string choices
    community: string
```

## Route policies

```yaml
RoutePolicy:
    name: string
    device: dcim.Device
    description: string
```

```yaml
RoutePolicyTerm:
    route_policy: cmdb.RoutePolicy
    description: string
    sequence: integer
    decision: string choices

    # match
    from_bgp_community: string
    from_bgp_community_list: cmdb.BgpCommunityList
    from_prefix_list: cmdb.PrefixList
    from_source_protocol: string
    from_route_type: string
    from_local_pref: integer

    # set
    set_local_pref: integer
    set_community: string
    set_origin: string
    set_metric: integer
    set_large_community: string
    set_as_path_prepend: string
    set_next_hop: IPaddress
```
