# Criteo SONiC utilities

## Installation

Our SONiC modules require some custom script to be installed:

* `/opt/salt/scripts/criteo_fdbshow`
* `/opt/salt/scripts/criteo_intf_information`

These scripts are available in the [Criteo SONiC utilities](https://github.com/criteo/criteo-sonic-utilities) repository.

You can use the provided [Salt state](https://github.com/criteo/criteo-sonic-utilities/blob/main/utilities/install.sls) to deploy them automatically. This state assumes some grains are properly set for each SONiC device:

```yaml
hwsku: some-hardware
nos: sonic
sonic_asic_type: some-asic
sonic_build_date: some-date
sonic_build_version: 202205
sonic_built_by: someone
sonic_commit_id: some-commit-id
```

!!! tip

    The needed grains are automatically set by our [SONiC Salt Deployer](SONiC-support/Deploy-Salt-minion).
