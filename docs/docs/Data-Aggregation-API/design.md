# Design

## Ingestors

An ingestor is a component responsible for fetching data from a single source of truth. Most of the time it is querying a single endpoint.

Examples of ingestor with their associated endpoints:

* BGPSessions: `/api/plugins/cmdb/bgp-sessions/`
* RoutePolicies: `/api/plugins/cmdb/route-policies/`

## Precompute

The precompute step is responsible for extracting raw data per device. The goal is to be able to have all needed data to compute one device configuration.

For instance, a `BGPSession` has two peers: `peer_a` and `peer_b`. The devices matching the peers will all have this `BGPSession` in their respective `raw_data`.

??? example

    Here is a simplified output of the `bgp-sessions` endpoint.

    ```json
        {
            "results": [
                {
                    "peer_a": {
                        "local_address": {
                            "address": "192.0.2.16/31"
                        },
                        "device": {
                            "name": "tor1"
                        },
                        "local_asn": {
                            "number": 65000,
                            "organization_name": "Criteo-65000"
                        },
                        "description": "to-spine1",
                    },
                    "peer_b": {
                        "local_address": {
                            "address": "192.0.2.17/31"
                        },
                        "device": {
                            "name": "spine1"
                        },
                        "local_asn": {
                            "number": 65001,
                        },
                        "description": "to-tor1",
                    },
                    "status": "active",
                    "password": "thisisanincredibleandcomplexpassword:)",
                }
            ]
        }
    ```

    The precompute will "copy" this structure for both devices:

    * `tor1.raw_data["bgp-session"] = results[0]`
    * `spine1.raw_data["bgp-session"] = results[0]`

    Thanks to this simple precompute part, `tor1` OpenConfig can be generated independently:

    ```py
    tor1.neighbor[0].local_as = tor1.raw_data["bgp-session"].peer_a.local_asn
    tor1.neighbor[0].remote_as = tor1.raw_data["bgp-session"].peer_b.local_asn
    ...
    ```

    > this is pseudo-code just to explain the idea.

## Convertors

This is where the magic happens. From the precomputed data, the Data Aggregation API generates OpenConfig configuration for each device.

Thanks to [ygot](https://github.com/openconfig/ygot) and [OpenConfig YANG models](https://github.com/openconfig/public), the Data Aggregation API has all the OpenConfig Go structures.

* OpenConfig structure is respected for sure.
* Logic is validated: if a neighbor refers to an undefined route policy, it will raise an error.
* Output: conversion to JSON RFC7951 is provided by ygot.
* All features from ygot can be implemented.

## Reporting

!!! warning

    :wrench: This section is under construction.

## Endpoints

!!! warning

    :wrench: This section is under construction.
