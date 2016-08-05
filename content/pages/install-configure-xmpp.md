Title: Install and configure XMPP
url: install-configure-xmpp
save_as: install-configure-xmpp.html
section: install
index: 1

Install and configure XMPP
==========
The Extensible Messaging and Presence Protocol (<a href="https://en.wikipedia.org/wiki/XMPP" target="_blank">XMPP</a>) is a protocol for message-oriented communication based on XML (Extensible Markup Language). In Fogbow we have two XMPP server components: the **Fogbow Manager** and the **Fogbow Rendezvous**. An XMPP server components is a software module capable of communicating with an XMPP server using the appropriate protocol.

## Install
Fogbow XMPP components are not tied to any particular server implementation. For the sake of simplicity, this document covers the installation and use of the [prosody](http://prosody.im/) XMPP server which has been used in Fogbow federations operated by the Fogbow team. To install it, in a debian-based distribution, run the following commands:
``` shell
$ apt-get update
$ apt-get install prosody
```

## Configure

For each new **Fogbow Manager** and **Fogbow Rendezvous** installed, it is necessary to add a new component to the `/etc/prosody/prosody.cfg.lua` configuration file. Also, the **Fogbow Manager** and **Fogbow Rendezvous** **xmpp_jid** and 
**xmpp_password** properties, specified in the [Install and Configure Fogbow Manager](http://www.fogbowcloud.org/install-configure-fogbow-manager#configure) section and in the [Install and Configure Fogbow Rendezvous](http://www.fogbowcloud.org/install-configure-fogbow-rendezvous#configure) section, respectively, should be used as the **component name** and **component_secret**, as shown below.

```bash
# Manager component
Component "my-manager.internal.mydomain"
        component_secret = "manager_password"
        
# Rendezvous component
Component "my-rendezvous.internal.mydomain"
        component_secret = "rendezvous_password"
```

## Run
After editing the prosody configuration, run the command below to restart it.
``` shell
$ service prosody restart
```
