# Configuration cleaning

## Current status

For now, AFK provides a partial configuration removal feature. It is not enabled by default.

We do not advise enabling it, as it has not been battle-tested yet. If you still want to use it, please be careful and run tests in your lab first!

## Target

The target is to manage the configuration like any Configuration Management Software, e.g. Chef.

The system maintains the desired state but will not remove extra configuration. For instance, when you maintain the configuration of a server with Chef, it will never remove an extra package installed by a user. This is the same here. If you add manually a protocol which is not supported by AFK, like OSPF, it will not remove it. Meanwhile, BGP sessions are maintained by AFK, so extra BGP session can be removed.

We do not want to give too much power to the CMDB, which hosts the source of truth of what should be on the device. We do not want to remove all BGP sessions if someone removes the wrong device in NetBox.

This is why we have already implemented safeguards and will continue to do so.

Some examples:

* BGP: keep a minimum of uplinks up and/or authorize the removal of down BGP sessions only.
* Route maps: only authorize the removal of unused route maps.
* No data at all for the device: do nothing. The device decommission process should be handled either manually or via a dedicated tooling/script.
