# Missing features in OpenConfig

## Context and approaches

OpenConfig models do not provide all existing network features.

There are three approaches to this:

1. changing the way to configure the devices
    * sometimes this is not possible
2. augmenting the OpenConfig models
    * only if the feature cannot be implemented another way
    * this would only be temporary
    * a PR to [OpenConfig](https://github.com/openconfig/public) is mandatory
3. doing a separate model
    * only in last resort

!!! warning

    We want to stick with OpenConfig standard models.

## Missing features

### BGP

| Feature | Decision |
|---------|----------|
| `enforce first-as` | Migrate to a route-map like `match as-path <asn>` <asn> being the AS of the neighbor |
| `network` | `to be defined, maybe via route-maps` |
| `soft reconfiguration inbound` | `to be defined` |

### Routing policies

| Feature | Decision |
|---------|----------|
| `large communities` | do a PR in OpenConfig repo |
| decision in prefix-list: `permit` / `deny` | all prefixes in prefix-lists are hardcoded as `permit`. The `deny` must be done at in the route-map |
| prefix-list sequence | hardcoded in the jinja loop |