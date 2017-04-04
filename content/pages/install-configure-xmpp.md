Title: Install and configure XMPP
url: install-configure-xmpp
save_as: install-configure-xmpp.html
section: install
index: 1

Install and configure XMPP
==========
The Extensible Messaging and Presence Protocol (<a href="https://en.wikipedia.org/wiki/XMPP" target="_blank">XMPP</a>) is a protocol for message-oriented communication based on XML (Extensible Markup Language). In Fogbow we have two XMPP server components: the **Fogbow Manager** and the **Fogbow Rendezvous**. An XMPP server component is a software module capable of communicating with an XMPP server using the appropriate protocol.

## Install
Fogbow XMPP components are not tied to any particular server implementation. For the sake of simplicity, this document covers the installation and use of the <a href="http://prosody.im/" target="_blank">prosody</a> XMPP server which has been used in Fogbow federations operated by the Fogbow team. To install it, in a debian-based distribution, run the following commands:
``` shell
$ apt-get update
$ apt-get install prosody
```

## Configure

For each new **Fogbow Manager** and **Fogbow Rendezvous** installed, it is necessary to add a new component to the `/etc/prosody/prosody.cfg.lua` configuration file. Also, the **Fogbow Manager** and **Fogbow Rendezvous** **xmpp_jid** and 
**xmpp_password** properties, specified in the [Install and Configure Fogbow Manager](http://www.fogbowcloud.org/install-configure-fogbow-manager#configure) section and in the [Install and Configure Fogbow Rendezvous](http://www.fogbowcloud.org/install-configure-fogbow-rendezvous#configure) section, respectively, should be used as the **component name** and **component_secret**, as shown below.

```bash
# If necessary, change the component-to-component port
# default is 5347
# component_ports={ 8888 }={ port_c2s }

# If necessary, change the server-to-server port
# default is 5269
# s2s_ports={ port_s2s }

# listen for connections on all interfaces
component_interface = "0.0.0.0"

# Manager component
Component "my-manager.internal.mydomain"
        component_secret = "manager_password"
        
# Rendezvous component
Component "my-rendezvous.internal.mydomain"
        component_secret = "rendezvous_password"
```

In addition to prosody configuration, it is also necessary to follow below networking constraints:

- In your DNS, associate the name of the xmpp component with the public IP of the machine where the XMPP was installed.

- Allow external access through the 5269 port (default server-to-server port) in your firewall.

- Allow intranet access through the 5347 (default component-to-component port) in your firewall.

## Run
Now, run the command below to restart the prosody server.
``` shell
$ service prosody restart
```
