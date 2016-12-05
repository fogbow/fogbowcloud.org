Title: Install and configure Fogbow Rendezvous
url: install-configure-fogbow-rendezvous
save_as: install-configure-fogbow-rendezvous.html
section: install
index: 4

Install and configure the Fogbow Rendezvous
==========

The **Fogbow Rendevouz** (FR) is distributed in two forms: as source code, or as a binary package for debian-based distributions. Choose the best distribution for your system, download it and install it as follows.

## Install from source
To get the lastest stable version of the component, download it from our repository:

``` shell
wget https://github.com/fogbow/fogbow-rendezvous/archive/master.zip
```

Then, decompress it:
``` shell
unzip master.zip
```

Now, install it with Maven:

```
cd fogbow-rendezvous-master
mvn install
```

## Install from debian package

Download a stable version from our <a href="http://downloads.fogbowcloud.org/stable/debian/">package repository</a> and install it with dpkg:

```bash
dpkg -i fogbow-rendezvous_$version.deb
```

## Configure
After the installation, move the file ```rendezvous.conf.example``` to ```rendezvous.conf``` and edit its contents as appropriate:
``` shell
# XMPP address of your Rendezvous Component
xmpp_jid=my-rendezvous.internal.mydomain

# XMPP component password
xmpp_password=rendezvous_password

# Address of the host running the XMPP server.
xmpp_host=IP_of_external.domain

# XMPP server port (to listen for components communication)
xmpp_port=5347

# The neighbors property indicates other existent rendezvous
# which this component can exchange information about Fogbow Managers liveness.
neighbors=neighbor1@domain1.com,neighbor2@domain2.com

# The properties below are used to limit, in size, the rendezvous' responses.
# Maximum number of managers that should be returned in a whoIsAlive response.
max_whoisalive_manager_count=100
# Maximum number of managers that should be returned in a whoIsAliveSyncresponse.
max_whoisalivesync_manager_count=100
# Maximum number of neighbors that should be returned in a whoIsAliveSyncresponse.
max_whoisalivesync_neighbor_count=100

# The properties below control "iamalive" mechanism
# The period in milliseconds between successive "iamalive" messages.
iamalive_period=30000
# How many "iamalive" messages should be lost before the Fogbow Rendezvous suspects
# the Fogbow Manager is not available.
iamalive_max_message_lost=3

# The property below indicates the WhiteList pluging used by the rendevouz.
white_list_class=org.fogbowcloud.rendezvous.core.plugins.whitelist.AcceptAnyWhiteListPlugin
``` 

Remember that the ```xmpp_jid``` and ```xmpp_password``` must match the values assigned when installing the XMPP server (refer to the [Install and configure XMPP](install-configure-xmpp.md) section of our documentation).

After adding the FR to your XMPP server, you need to add a new entry in your DNS to resolve the FR name to the XMPP server IP address, as shown in the example below.
``` shell
my-rendezvous.internal.mydomain        22      IN      A       IP_of_external.domain
```

## Configure LOG

Rename the file ```log4j.properties.example``` to ```log4j.properties``` and edit it according your necessity.

```bash
# Root logger option
log4j.rootLogger=DEBUG, file

# Direct log messages to a log file
log4j.appender.file=org.apache.log4j.RollingFileAppender
# log path
log4j.appender.file.File=/var/log/fogbow-rendezvous/fogbow-rendezvous.log
log4j.appender.file.MaxFileSize=10MB
log4j.appender.file.MaxBackupIndex=10
log4j.appender.file.layout=org.apache.log4j.PatternLayout
log4j.appender.file.layout.ConversionPattern=%d{yyyy-MM-dd HH:mm:ss} %-5p %c{1}:%L - %m%n

# Different log levels for restlet and http-client
log4j.category.org.restlet=INFO

```

## Run
To start the FR, run the ```start-rendezvous``` script inside ```./bin```.
``` shell
./bin/start-rendezvous
```
