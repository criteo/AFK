# Configuration cleaning

## Current status

Currently, AFK provides a partial configuration removal feature. It is not enabled by default.

We do not advise enabling it as it has not been battle tested yet. If you still want to use it, please be careful and do tests in your lab first!

## Target

The target is to manage the configuration like any Configuration Management Software: only delete what it set.

For instance, a BGP session can be set and edited, so it can be removed.

We do not want to give too much power to the CMDB, which hosts the source of truth of what should be on the device. We do not want to remove all BGP sessions if someone removes the wrong device in NetBox.

This is why we have already implemented safeguards and will continue to do so.

Some examples:

* BGP: keep a minimum of uplinks up and/or authorize to remove down BGP sessions only
* Route maps: only authorize the removal of unused route maps
* no data at all for the device: do nothing. The device decommission process should be handled either manually or via a dedicated tooling/script
