Title: Opportunism
url: opportunism
save_as: opportunism.html
section: arch
index: 2

Opportunism
==========
Opportunism is a feature that allows shared resources to be used as processing nodes of a private infrastructure-as-a-service cloud. Each shared resource can be configured to define when it is deemed usable by the cloud, <I>e.g.</I> when it is idle, or when the utilization is below some threshold. When the resources are in a state that allows them to be used by the cloud, then they show up to the cloud orchestrator and can be used to have VM instances allocated to them, otherwise they are removed from the cloud, in which case, any VM instance that is still running in the resource is either suspended or simply terminated.

A <B>Fogbow Manager</B> can be used to federate an opportunistic cloud with other private clouds that also use shared resources in an opportunistic way, or that use dedicated resources in the conventional way.

## Implementation

In the current implementation it is possible to create opportunistic private clouds managed by either the <B>OpenStack</B>) or the <B>CloudStack</B> orchestrators, although extending the software to support other orchestrators is trivial. The opportunistic cloud uses desktops in a LAN as computing nodes. We provide support for desktops running both Linux and Windows.

### Monitoring
The monitoring is implemented using PowerNap, a configurable deamon that executes some actions when the system is idle. The idleness is determined by monitors that can be configured to watch CPU load, processes, mouse/keyboard, etc. For instance, one can configure PowerNap in such a way that resources are deemed usable by the cloud when there is no mouse/keyboard activity for a configurable period of time.

PowerNap allows the customization of an action script that determines the action to be executed when the system is deemed idle. However, it does not have a way to define the action executed when the system is back in activity. Also, PowerNap only triggers the recovery action when running in powersave mode (ACTION_METHOD=0). Thus, we have modified PowerNap to allow the definition of a customized recovery action script. A new property named RECOVER_ACTION was added to the configuration file through which the user can set the path to the action script, and the module powernap.py was modified to load the value of this property. We have also modified the PowerNap daemon to use the recovery action in the best effort mode (ACTION_METHOD=4), that allows for customized actions.

PowerNap has some monitors to check if the host is idle, but the monitor that checks for input devices (mouse and keyboard) just watches USB devices, so a new monitor named PS2Monitor was added to also check PS2 devices.

With those changes, PowerNap is able to be used in the fogbow opportunistic component that allows shared resources to be used in a private cloud.

### Action
The action part is implemented by two scripts (start-node and stop-node), one to start the compute service of the underlying cloud and another to stop it. We have scripts implementd for CloudStack and OpenStack. Typically, the start-node action script just enables network and compute services, while the stop-node action script looks for active VMs, stops them and disables the network and compute services.
