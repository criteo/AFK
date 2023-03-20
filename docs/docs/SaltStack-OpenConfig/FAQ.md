# FAQ

## Why are the templates minimalist, with little logic and no inheritance?

The idea here is to make the maximum of logic in Python, so on the Salt module side.

We want to avoid complex Jinja for a lot of reasons: difficult to read, to maintain, to debug, to add features etc... Also, it makes more sense to handle most of the logic with a programming language rather than in templates.

We also only authorize two levels of indentation in the template. If you need more, split your templates.

## State modules are supposed to be agnostic, and Execution module are not. Why not use Execution modules to handle Network OS particularity?

The only answer: it is in development and for now it brings more complexity to create Execution modules to handle configuration differences.

However, we do use Execution modules to get information from the device or to apply the configuration.

## EOS/SONiC: why negate all configuration command and not push diffs or override everything?

The easiest option to answer is because all OS does not provide a way to obtain a diff (FRR on SONiC for instance).

We cannot override the entire configuration because a section of the configuration might be managed by multiple states. Example: one state to manage global BGP configuration, one for the BGP sessions, one for EVPN.

Sometimes we do negate the entire section. We remove a route-map to recreate it entirely on EOS because it is safer and easier to do that: we are sure the terms of the route-map does not contain extra configuration we don't support/want.

In the end, our goal is to maintain a state. It does not mean we will remove extra configuration. You can compare this to Chef: it does not remove a package manually installed by someone. But it enforces some packages to be installed with specific versions and configuration.

## What is the behavior of SONiC BGP configuration if something bad happens?

FRR:

If a command is incorrect, it is ignored and the rest of the configuration is applied. It gives some errors in the output.

!!! example

    ```
    line 11: Failure to communicate[13] to zebra, line: ip prefix-list PF-LOOPBACK seq 10 permit 10.252.200.0/22 ge 22
    % Invalid prefix range for 10.252.200.0/22, make sure: len < ge-value <= le-value
    ```

If we attempt to remove a statement which is not present, it gives an error. But it still apply the rest of the config.

!!! example

    ```
    % Could not find route-map RM-CLOS-IN
    line 13: Failure to communicate[13] to zebra, line: no route-map RM-CLOS-IN
    These are not caught by the dry-run feature of vtysh, which only checks if the config is semantically correct.
    ```

It means we need to cover all possible incorrect configuration before trying to apply it.

## FRR is not (yet) offering a commit base configuration, how do we handle this?

This part is complex. The future is clear: the Northbound API. We plan to use the future incremental feature via CLI or directly gRPC.

But this feature is not ready yet on FRR side.

In the meantime, we will make all the changes in real time:

* Salt will first disable "event-driven" notification to Zebra when there is a policy to update (using `bgp route-map delay-timer 0`)
* then the changes are applied
* "event-driven" notification to Zebra are re-enabled

This is the safest option we have for now. All of this happens in a really short time as VTYSH is pushing all lines in a simple `for loop` (in C).
