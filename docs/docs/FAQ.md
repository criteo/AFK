# FAQ

## How to decommission an entire device?

For now, the mantra is: create your own state to decommission.

We do have an optional removal feature but with some safeguards.

As of today, we think decommission should be handled in different states and modules than provisioning.

## What part is device-agnostic, and what is not?

The only part of the stack which is not agnostic is SaltStack.

The job of Salt is to convert agnostic data coming from Data Aggregation API to a configuration the devices understand.
So, the data in the NetBox CMDB and the Data Aggregation API must be completely agnostic, with nothing specific to a Network OS.

If we do not do that, Salt could try to push a configuration which would not be appropriate for the device.

!!! note

    The Data Aggregation API might stop to be device-agnostic because of interface naming. More info to come.

## What if I have a use case which is not covered by OpenConfig?

The rules are:

* ask yourself: is my use case really necessary? Or can I adapt?
* try to find a configuration which is supported by OpenConfig
* if your workflow is really a needed corner case:
    * do a dedicated plugin in NetBox
    * do a dedicated plugin in the Data Aggregation API (feature not yet available) to provide a separate custom model
    * do a dedicated state for your use case
    * ensure you have the necessary safeguard to avoid your configuration being removed by another state

See also [Data-Aggregation-API](Data-Aggregation-API/missing-features-in-openconfig.md).

## Some Network OS requires extra configuration for a service, where do I put that?

You are perfectly allowed to duplicate/convert an object if needed.

For instance, depending on the Network OS, PBR sometimes requires an ACL and sometimes a route-map. If it is doable, do that conversion in the Salt module and/or in the template.
