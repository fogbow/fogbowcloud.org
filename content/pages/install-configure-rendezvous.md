Title: Install and configure fogbow rendezvous
url: install-configure-rendezvous
save_as: install-configure-rendezvous.html
section: install-configure
index: 6

Install and configure rendezvous
==========

The rendezvous component acts as a discovery service for a fogbow federation. It allows members to find each other by providing a complete status of the federation.

## Install from source
Get the latest code of the project.
``` shell
git clone https://github.com/fogbow/fogbow-rendezvous.git
```
Then, install it
``` shell
mvn install
```

## Install from debian package
With an XMPP server already installed and configured, download to the <a href="http://downloads.fogbowcloud.org/nightly/debian/fogbow-rendezvous/fogbow-rendezvous_latest.deb" target=_blank>latest debian package</a>
```bash
wget http://downloads.fogbowcloud.org/nightly/debian/fogbow-rendezvous/fogbow-rendezvous_latest.deb
```

Then, install it with dkpg
```bash
dpkg -i fogbow-rendezvous_latest.deb
```

As the rendezvous runs as an XMPP component, you need an XMPP server running and properly configured.
We recommend <a href="https://prosody.im/" target="_blank">prosody</a> due to its ease of configuration.

If you are using Prosody, you can add a component to its configuration with:
``` shell
Component "rendezvous.test.com"
       component_secret = "password"
```
# Actions before configure
After add the component to your XMPP server, you need to add a new entry in your DNS to resolve your component name to a IP address like in example below. It is needed for all XMPP components such as rendezvous and Fogbow manager.
``` shell
rendezvous.test.com.        22      IN      A       199.27.76.133
```

## Configure
After the installation, move the file ```rendezvous.conf.example``` to ```rendezvous.conf``` and edit its contents:
``` shell
# XMPP address of your Rendezvous Component, as configured in the XMPP server.
xmpp_jid=rendezvous.test.com

# XMPP component password, as configured in the XMPP server.
xmpp_password=password

# Address of the host running the XMPP component.
xmpp_host=127.0.0.1

# Port in which the XMPP server will be listening for components, as configured in the XMPP server.
xmpp_port=5347
```

The property below references a list of ids of other existent rendezvous, with whom this component can exchange information about managers alive and keep itself updated. This is part of the [replication strategy](http://www.fogbowcloud.org/rendezvous) used to avoid system crashes. 
``` shell
# Known Rendezvous Neighbor aderesses.
neighbors=neighbor1@test.com,neighbor2@test.com
```

The properties below are part the [Result Set Management strategy](http://www.fogbowcloud.org/rendezvous) used to limit, in size, the rendezvous' responses.
``` shell
# Maximum number of managers that should be returned in a whoIsAlive response.
max_whoisalive_manager_count=100

# Maximum number of managers that should be returned in a whoIsAliveSyncresponse.
max_whoisalivesync_manager_count=100

# Maximum number of neighbors that should be returned in a whoIsAliveSyncresponse.
max_whoisalivesync_neighbor_count=100
```

The properties below reference to message "iamalive" sent from Fogbow Manager.
``` shell
# How many time the Fogbow Manager send the message "iamalive" in miliseconds.
iamalive_period=30000
# How many times the Fogbow Rendezvous wait the message "iamalive".
iamalive_max_message_lost=3
```

The property below reference which members are allowed by Fogbow Rendezvous.
``` shell
white_list_class=org.fogbowcloud.rendezvous.core.plugins.whitelist.AcceptAnyWhiteListPlugin
``` 

## Run
To start the rendezvous component, run the start-rendezvous script inside ```./bin```.
``` shell
./bin/start-rendezvous
```