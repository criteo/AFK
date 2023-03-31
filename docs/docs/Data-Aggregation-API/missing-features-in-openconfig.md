# Missing features in OpenConfig

## Context and approaches

OpenConfig models do not provide all existing network features.

There are three approaches to this:

1. Changing the way to configure the devices.
    * Sometimes this is not possible.
2. Augmenting the OpenConfig models.
    * Only if the feature cannot be implemented another way.
    * This would only be temporary.
    * A PR to [OpenConfig](https://github.com/openconfig/public) is mandatory.
3. Doing a separate model.
    * This should only be a last resort.

!!! warning

    We want to stick with OpenConfig standard models.

## Missing features

### BGP

| Feature | Decision |
|---------|----------|
| `enforce first-as` | Migrate to a route-map like `match as-path <asn>` <asn> being the AS of the neighbor. |
| `network` | `To be defined, maybe via route-maps` |
| `soft reconfiguration inbound` | `To be defined` |

### Routing policies

| Feature | Decision |
|---------|----------|
| `large communities` | Do a PR in OpenConfig repo |
| decision in prefix-list: `permit` / `deny` | All prefixes in prefix-lists are hardcoded as `permit`. The `deny` must be done at in the route-map. |
| prefix-list sequence | Hardcoded in the jinja loop |