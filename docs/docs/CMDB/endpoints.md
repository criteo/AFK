# Endpoints

## ASNs

Endpoint: `/api/plugins/cmdb/asns/`

Provides all available ASN.

??? example

    ```json
    {
        "results": [
            {
                "id": 1,
                "created": "2022-09-28T12:28:52.112949Z",
                "last_updated": "2022-09-28T12:28:52.112978Z",
                "organization_name": "Paris",
                "number": 65000
            }
        ]
    }
    ```

## BGP Global

Endpoint: `/api/plugins/cmdb/bgp-global/`

Provides the BGP global configuration for each device.

??? example

    ```json
    TODO
    ```


## BGP sessions

Endpoint: `/api/plugins/cmdb/bgp-sessions/`

Provides all BGP sessions.

??? example

    ```json
    {
        "results": [
            {
                "id": 1,
                "peer_a": {
                    "id": 1,
                    "local_address": {
                        "id": 1234,
                        "url": "https://netbox.local/api/ipam/ip-addresses/1234/",
                        "display": "192.0.2.16/31",
                        "family": 4,
                        "address": "192.0.2.16/31"
                    },
                    "device": {
                        "id": 123,
                        "name": "tor1"
                    },
                    "local_asn": {
                        "id": 1,
                        "number": 65000,
                        "organization_name": "Criteo-65000"
                    },
                    "afi_safis": [
                        {
                            "id": 217,
                            "route_policy_in": null,
                            "route_policy_out": null,
                            "afi_safi_name": "ipv4-unicast"
                        }
                    ],
                    "route_policy_in": null,
                    "route_policy_out": null,
                    "created": "2022-11-07T16:57:33.848779Z",
                    "last_updated": "2022-11-07T16:57:33.848795Z",
                    "description": "to-spine1",
                    "enforce_first_as": true,
                    "maximum_prefixes": 10000
                },
                "peer_b": {
                    "id": 218,
                    "local_address": {
                        "id": 22,
                        "url": "https://netbox.local/api/ipam/ip-addresses/22/",
                        "display": "192.0.2.17/31",
                        "family": 4,
                        "address": "192.0.2.17/31"
                    },
                    "device": {
                        "id": 345,
                        "name": "spine1"
                    },
                    "local_asn": {
                        "id": 10,
                        "number": 65001,
                        "organization_name": "Criteo-65001"
                    },
                    "afi_safis": [
                        {
                            "id": 2,
                            "route_policy_in": null,
                            "route_policy_out": null,
                            "afi_safi_name": "ipv4-unicast"
                        }
                    ],
                    "route_policy_in": null,
                    "route_policy_out": null,
                    "created": "2022-11-07T16:57:33.853487Z",
                    "last_updated": "2022-11-07T16:57:33.853500Z",
                    "description": "to-tor1",
                    "enforce_first_as": true,
                    "maximum_prefixes": 10000
                },
                "tenant": null,
                "created": "2022-11-07T16:57:33.856145Z",
                "last_updated": "2022-11-07T16:57:33.856156Z",
                "status": "active",
                "password": "",
                "circuit": null
            },
        ]
    }
    ```


## Prefix lists

Endpoint: `api/plugins/cmdb/prefix-lists/`

Provides the prefix lists for each device.

??? example

    ```json
    {
        "results": [
            {
                "id": 129,
                "name": "PF-ANY_IPV6",
                "device": {
                    "id": 1,
                    "name": "tor1"
                },
                "ip_version": "ipv6",
                "terms": [
                    {
                        "sequence": 10,
                        "decision": "permit",
                        "prefix": "::/0",
                        "le": 128,
                        "ge": null
                    }
                ]
            },
        ]
    }
    ```


## BGP community lists

Endpoint: `/api/plugins/cmdb/bgp-community-lists/`

Provides the BGP community lists for each device.

??? example

    ```json
    {
        "results": [
            {
                "id": 1,
                "device": {
                    "id": 1,
                    "name": "tor1"
                },
                "terms": [
                    {
                        "sequence": 10,
                        "decision": "permit",
                        "community": "65000:10000"
                    }
                ],
                "created": "2022-07-28T13:37:05.699938Z",
                "last_updated": "2022-07-28T13:37:05.699968Z",
                "name": "CL-SERVER"
            }
        ]
    }
    ```

## Route policies

Endpoint: `/api/plugins/cmdb/route-policies/`

Provides the route maps for each device.

??? example

    ```json
    {
        "results": [
            {
                "id": 129,
                "name": "RM-UPLINK-IN",
                "device": {
                    "id": 123,
                    "name": "tor1"
                },
                "description": "tor1:uplink-in",
                "terms": [
                    {
                        "description": "",
                        "sequence": 10,
                        "decision": "permit",
                        "from_bgp_community": "",
                        "from_bgp_community_list": null,
                        "from_prefix_list": {
                            "id": 129,
                            "device": 123,
                            "name": "PF-ANY_IPV6"
                        },
                        "from_source_protocol": "",
                        "from_route_type": "",
                        "from_local_pref": null,
                        "set_local_pref": null,
                        "set_community": "",
                        "set_origin": "",
                        "set_metric": null,
                        "set_large_community": "",
                        "set_as_path_prepend": "",
                        "set_next_hop": null
                    },
                    {
                        "description": "",
                        "sequence": 20,
                        "decision": "permit",
                        "from_bgp_community": "",
                        "from_bgp_community_list": {
                            "id": 1,
                            "device": 123,
                            "name": "CL-SERVER"
                        },
                        "from_prefix_list": null,
                        "from_source_protocol": "",
                        "from_route_type": "",
                        "from_local_pref": null,
                        "set_local_pref": null,
                        "set_community": "",
                        "set_origin": "",
                        "set_metric": null,
                        "set_large_community": "",
                        "set_as_path_prepend": "",
                        "set_next_hop": null
                    },
                    {
                        "description": "",
                        "sequence": 30,
                        "decision": "deny",
                        "from_bgp_community": "",
                        "from_bgp_community_list": null,
                        "from_prefix_list": null,
                        "from_source_protocol": "",
                        "from_route_type": "",
                        "from_local_pref": null,
                        "set_local_pref": null,
                        "set_community": "",
                        "set_origin": "",
                        "set_metric": null,
                        "set_large_community": "",
                        "set_as_path_prepend": "",
                        "set_next_hop": null
                    }
                ]
            }
        ]
    }
    ```