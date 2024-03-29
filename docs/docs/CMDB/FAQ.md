# FAQ

## Why not use existing models from NetBox?

[NetBox](https://netbox.dev) is a great tool to manage your DCIM and IPAM.

Our CMDB models directly use the DCIM and IPAM models, like `Device` and `IP prefixes`.

NetBox also includes several models which could be considered as CMDB such as `VLAN`, `VRF` and `L2VPN`. We do not want to use these models to avoid confusion and design mismatches with our CMDB.

## Why not implement directly OpenConfig models in the CMDB?

OpenConfig models are designed from a device perspective.

Our CMDB models are designed from a service/asset perspective. It allows us to apply common parameters without worrying of applying them at both ends.

Example: we can directly set a maintenance status on a BGP session.

It also allows us to limit redefinition of values that can lead to misconfiguration. For instance, in OpenConfig nothing prevents us from having a mismatch of ASN in a BGP session. In our CMDB, it cannot happen as the BGP session is the central object which links two devices, hence the neighbor A remote-as is the neighbor B local-as.

## Is it perfect and completely generic?

No :innocent:

It will not cover all use cases, but we will bring more features.

The CMDB models come from opinionated choices aiming to simplify the implementation.

Also, we do not aim to have a factorized configuration.

## Why is max-prefixes configured at the top level of the neighbor and not in a SAFI?

This is due to an EOS limitation. It cannot set the max prefix at SAFI level:

```
device.lab(config)#router bgp 65000

device.lab(config-router-bgp)#neighbor 100.64.1.0 maximum-routes ?
  <0-4294967294> Maximum number of routes (0 means unlimited)

device.lab(config-router-bgp)#address-family ipv4

device.lab(config-router-bgp-af)#neighbor 100.64.1.0 ?
  activate Activate neighbor in the address family
  additional-paths BGP additional-paths commands
  default-originate Advertise a default route to this peer
  graceful-restart Enable graceful restart mode
  next-hop Next-hop address-family configuration
  next-hop-unchanged Preserve original nexthop while advertising routes to eBGP peers
  prefix-list Prefix list reference
  route-map Name a route map
  weight Assign weight for routes learnt from this peer
```

## Why is there no permit/deny field in community-lists and prefix-lists?

While some devices provides a `permit/deny` field to a community/prefix-list, other Network OS like JunOS does not. More importantly, OpenConfig does not have it either.

If you are currently using `permit/deny`, we suggest you to adapt your route-map instead.

!!! example

    You should migrate from:
    ```
    ip prefix-list FANCY_PREFIX_LIST 10 deny 10.0.0.0/24
    ip prefix-list FANCY_PREFIX_LIST 20 permit 10.0.0.0/8 le 32

    route-map RM-TEST-IN 5 permit
      match ip prefix-list FANCY_PREFIX_LIST
    ```

    to:
    ```
    ip prefix-list FANCY_PREFIX_LIST 10 permit 10.0.0.0/24
    ip prefix-list ANOTHER_FANCY_PREFIX_LIST 20 permit 10.0.0.0/8 le 32

    route-map RM-TEST-IN 5 deny
      match ip prefix-list FANCY_PREFIX_LIST

    route-map RM-TEST-IN 10 permit
      match ip prefix-list ANOTHER_FANCY_PREFIX_LIST
    ```

## Why cannot a route-map filter on SAFI while OpenConfig permits it?

The issue here is coming from implementation differences between JunOS and EOS/FRR:

* In JunOS, this should be done using `afi-safi-in` in routing-policy (applying a route-map in a specific SAFI in BGP configuration is not possible):
  `route-map->term->from->safi`

* In EOS/FRR, this should be done in BGP config (`afi-safi-in` in routing-policy is not supported):
  `bgp->neighbor->safi->route-map`

Authorizing both methods could lead to conflicts.

!!! example

    If we set the following configuration in the CMDB:

    ```
    route-map RM-TEST
        term 10
            from afi-safi-in ipv4_unicast

    router bgp
        neighbor toto
        address-family ipv6 unicast
            route-map out RM-TEST
    ```

    * JunOS would only consider ipv4_unicast parameter
    * EOS/FRR would only consider ipv6_unicast parameter

    We would end up with a route-map with different meanings depending on the network OS.

We decided to not support `afi-safi-in` in routing-policy to simplify and reduce risk.

## Do we support route-maps applied in a specific SAFI and globally?

Yes.

* EOS: can set up different route-maps at both peer and SAFI level.
* FRR: can only set up route-map at SAFI level.
    * However, our Salt implementation uses a fallback mechanism:
        * A route-map defined at the SAFI level has the highest priority.
        * If there is no route-map at the SAFI level, then it applies the route-map provided at the peer level (if it exists).
* JunOS: does not permit having different route-map depending on the SAFI. A route-map is applied on all SAFI the neighbor is in. However, we have implemented a workaround at Salt level:
    * Salt duplicates the route-maps for all SAFI.
    * It associates the right route-map to the neighbor depending on which SAFI it is in.


!!! example "JunOS example"

    ```
    # we create duplicates per SAFI, adding "from family <safi>" in each term:
    set policy-options policy-statement AUTOGENERATED::RM-TEST:IPV4_UNICAST term 10 from family inet
    ...

    set policy-options policy-statement AUTOGENERATED::RM-TEST:IPV6_UNICAST term 10 from family inet6
    ...

    # then we apply the one matching the SAFI used in the CMDB neighbor configuration (bgp->neighbor->address-family->route-map)
    set routing-instance prod protocols bgp group TEST neighbor 192.0.2.1 export [AUTOGENERATED::RM-TEST:IPV4_UNICAST]
    ```

## Why is send-community not implemented?

For two reasons:

* We do not see the added value to have this option as it is enabled by default on most Network OS.
* Some OS like JunOS do not have such implementation: in that case, communities must be deleted in the route-map.

Adding it would make the Salt templates more complex because sometimes it would be directly in the BGP configuration, sometimes it would imply the creation of a route-map.

Additionally, it would make route-policies and BGP states more tightly coupled.

## Why are peer-groups deprecated / not supported?

Because peer-groups bring a lot of complexity and risks, especially because of FRR.

In FRR, for sessions being in a peer-group:

* The peer-group remote-as is not mandatory, therefore the neighbors must have a remote-as.
* If the peer-group has a remote-as, the neighbors in the peer-group cannot have a remote-as explicitly.

!!! note "some of the issues we found in FRR"

    When the neighbor is in a peer-group and the remote-as is set at neighbor level only:

    * Removing the neighbor of a peer-group removes entirely the neighbor.
    * Setting the remote-as at the peer-group level breaks BGP sessions which do not have the same remote-as.
    * Changing the peer-group of a neighbor is impossible: `error: “Cannot change the peer-group. Deconfigure first”`

    When the neighbor is in a peer-group and the remote-as is set at the peer-group level only:

    * Changing the remote-as of a single neighbor is not possible.
    * Removing the remote-as of the peer-group removes the entire peer-group configuration and its neighbors.

    There are other issues, and we did not even talk about the difference with the other Network OS...

## Questions about deprecated features

??? abstract "expand..."

    ### Why do peer-groups not have maximum-prefixes?

    * OpenConfig: `maximum-prefixes` can only be set at the SAFI level
    * FRR: only in SAFI
    * EOS: only at the peer/peer-group level
    * JunOS: only in SAFI

    Because of EOS, the `maximum-prefixes` field in the CMDB is set at the peer/peer-group level too. Then, the Data Aggregation API duplicates the value for each `maximum-prefixes` set.

    We could force the `maximum-prefixes` for each SAFI whether they are enabled or not. But in JunOS it would mean enabling all the SAFIs for the peer-group.

    So it is easier to enable this option only at the peer level, as the peer must be in a SAFI to work.

    ### Why don't we support SAFI in peer-groups?

    Because of FRR:

    - Neighbor de/activation in a SAFI is ignored if its peer-group is de/activated in this SAFI.
    - When removing a SAFI from peer-group, the neighbor’s SAFI changes.
    - When a neighbor’s SAFI list changes, the BGP session is reset.

    For retro-compatibility purposes, we do not manage the statement `no neighbor <peer-group> activate`. Meaning, if it is added manually, it will not be removed.

    ### Why is the remote-as mandatory in the peer-groups, and why must it have the same remote-as than its neighbor?

    !!! warning

        To ease migration, our Salt template for SONiC supports peer-groups without remote-as.

    All these restrictions come from FRR. In FRR, we cannot setup the remote-as at both peer-group and neighbor level.

    It means that the peer-group remote-as MUST match with all its neighbors remote-as. Otherwise, the CMDB would not reflect what would really be applied to the device.

    !!! tip "TL;DR"

        * Cannot have a remote-as set on both neighbor and its peer-group (error message: % Peer-group member cannot override remote-as of peer-group).
        * Migrating remote-as from neighbor to peer-group works, without BGP reset.
        * Migrating remote-as from peer-group to neighbor does not work. The neighbor gets deleted and recreated.

    Note: for local-as the behavior is different:

    * Both neighbor and peer-group can have a local-as set, different or the same.
    * On both neighbor and peer-group: we cannot set the same local-as as the global-as (error message: % Cannot have local-as same as BGP AS number).

    ```
    router bgp 65000
        neighbor PG-L3_RA local-as 65000
    % Cannot have local-as same as BGP AS number

        neighbor 192.0.2.1 local-as 65001
        neighbor 192.0.2.1 local-as 65000
    % Cannot have local-as same as BGP AS number
        neighbor 192.0.2.1 local-as 65002
    ```