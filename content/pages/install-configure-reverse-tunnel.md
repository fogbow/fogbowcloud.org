Title: Install and configure Fogbow Reverse Tunnel
url: install-configure-reverse-tunnel
save_as: install-configure-reverse-tunnel.html
section: install
index: 2

Install and configure the Fogbow Reverse Tunnel
==========

The reverse tunneling service provides public IP access to virtual machines created in the private cloud, when the local cloud offers only private IPs to these virtual machines. The Fogbow Reverse Tunnel is distributed in two forms: as source code, or as a binary package for debian-based distributions. Choose the best distribution for your system, download it and install it as follows.

## Pre-installation 

```bash
# If not installed previously
apt-get install maven
# If not installed previously
apt-get install openjdk-7-jdk
```

## Install from source
To get the lastest stable version of the component, download it from our repository:

``` shell
wget https://github.com/fogbow/fogbow-reverse-tunnel/archive/master.zip
```

Then, decompress it:
``` shell
unzip master.zip
```

Now, install it with Maven:

```
cd fogbow-reverse-tunnel-master
mvn install -Dmaven.test.skip=true
```

## Install from debian package

Download a stable version from our <a href="http://downloads.fogbowcloud.org/stable/debian/">package repository</a> and install it with dpkg:

```bash
dpkg -i fogbow-reverse-tunnel_$version.deb
```

## Configure
After the installation, rename the file ```reverse-tunnel.conf.example``` to ```reverse-tunnel.conf``` and, if necessary, edit its contents. Below we list the default values:
```bash
http_port=2223
tunnel_port_range=2224:4224
ports_per_ssh_server=5
external_port_range=20000:30000
host_key_path=hostkey.ser
idle_token_timeout=86400
check_ssh_servers_interval=120
```

The ```http_port``` property defines the port used to run the HTTP service of the Reverse Tunnel. This service handles requests to create and get tunneled ports. This port must be accessible by the Fogbow Manager and by the virtual machines, created by the Fogbow Manager.

The reverse tunnel service employs multiple tunnel servers to balance the service load. This service allocates one port to each of these servers. The ```tunnel_port_range``` property defines the range of servers' ports. This range of ports must be accessible by the Fogbow Manager and by the virtual machines, created by the Fogbow Manager.

The ```ports_per_ssh_server``` property defines the number of ssh ports to be allocated in each server.

The ```external_port_range``` property defines the range of public tunneled ports. The ports within this range must be accessible from external networks.

The ```host_key_path``` property defines the path to ssh key to be generated by the SSH Server. Setting the key path is important to have the SSH Server using the same key after restarting. The host key is used to make sure that the server is always the same when establishing new connections.

The ```idle_token_timeout``` property defines the expiration time of a tunnel port token. A token is associated with tunneled port each tunneled. The token expires if it is not used for the past ```idle_token_timeout``` seconds. After expiration, the port is reclaimed and can be used by another request.

The ```check_ssh_servers_interval``` property defines the period interval in seconds to verify token expirations.

## Configure LOG

Rename the file ```log4j.properties.example``` to ```log4j.properties``` and edit it according your necessity.

```bash
# Root logger option
log4j.rootLogger=DEBUG, file

# Direct log messages to a log file
log4j.appender.file=org.apache.log4j.RollingFileAppender
# log path
log4j.appender.file.File=/var/log/fogbow-reverse-tunnel/fogbow-reverse-tunnel.log
log4j.appender.file.MaxFileSize=10MB
log4j.appender.file.MaxBackupIndex=10
log4j.appender.file.layout=org.apache.log4j.PatternLayout
log4j.appender.file.layout.ConversionPattern=%d{yyyy-MM-dd HH:mm:ss} %-5p %c{1}:%L - %m%n

# Different log levels for restlet and http-client
log4j.category.org.restlet=INFO

```

## Run
To start the reverse tunnel server, run the ```start-tunnel-server``` script inside the root directory of the project.
``` shell
bash start-tunnel-server
```
