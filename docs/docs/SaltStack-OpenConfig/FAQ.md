# FAQ

## Why are the templates minimalist, with little logic and no inheritance?

The idea is to have most of the logic in Salt modules.

We want to avoid making Jinja templates more complex because:

* Jinja templates are difficult to read, maintain, debug and to add features.
* It makes more sense to handle most of the logic with a programming language rather than a template.

As another rule, we only authorize two levels of indentation in the template. If you need more, split your templates.

## State modules are supposed to be agnostic. Why not use Execution modules to handle Network OS specificities?

For now, it brings more complexity to create Execution modules to handle configuration differences.

However, we do use Execution modules to get information from the device and to apply the configuration.

## EOS/SONiC: why negate all configuration statements instead of pushing diffs or override everything?

Because not all Network OS provide a way to obtain a diff, e.g. SONiC/FRR.

We cannot override the entire configuration because a section of the configuration might be managed by multiple Salt States. For instance one State manages the global BGP configuration, another manages the BGP sessions and a third one manages EVPN.

Sometimes we do negate the entire section. We remove a route-map to recreate it entirely because it is safer and easier to do that: we ensure the route-map terms do not contain extra configuration we do not support.

## What is the behavior of SONiC BGP configuration if something bad happens?

If a command is incorrect, it is ignored by FRR and the rest of the configuration is applied. It returns errors in the output.

!!! example

    ```
    line 11: Failure to communicate[13] to zebra, line: ip prefix-list PF-LOOPBACK seq 10 permit 10.252.200.0/22 ge 22
    % Invalid prefix range for 10.252.200.0/22, make sure: len < ge-value <= le-value
    ```

If we attempt to remove a statement which is not present, it returns an error. But it still applies the rest of the configuration.

!!! example

    ```
    % Could not find route-map RM-CLOS-IN
    line 13: Failure to communicate[13] to zebra, line: no route-map RM-CLOS-IN
    These are not caught by the dry-run feature of vtysh, which only checks if the config is semantica:ly correct.
    ```

## FRR is not offering a commit-based configuration yet, how do we handle this?

This part is complex. The future of FRR is clear: the Northbound API. We plan to use the future incremental feature via CLI or gRPC.

In the meantime, AFK applies all changes in real time:

1. Salt disables "event-driven" notifications to Zebra when there is a policy to update, using `bgp route-map delay-timer 0`.
2. Changes are applied.
3. "Event-driven" notifications to Zebra are re-enabled.

Everything happens in a really short time as FRR pushes all lines in a simple `for loop` in C.
