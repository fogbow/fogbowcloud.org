Title: Install and configure Fogbow CLI
url: install-configure-fogbow-cli
save_as: install-configure-fogbow-cli.html
section: install
index: 5

Install and configure the Fogbow CLI
==========

The **Fogbow Manager** (FM) is controlled via a **Command Line Interface** (CLI) that makes easier for the fogbow users to get information about the federation members, images, quota, resources(compute, volume, network), and to manage the lifecycle of those resources. 

## Pre-installation 

```bash
# If not installed previously
apt-get install maven
# If not installed previously
apt-get install openjdk-8-jdk
# If not installed previously
apt-get install git
```

## Install from git

First, is necessary to download fogbow manager and install it with maven.
```shell
git clone https://github.com/fogbow/fogbow-manager-core.git
cd fogbow-manager-core
mvn install -Dmaven.test.skip=true
``` 

Now, to download the fogbow-cli and install it.
```shell
cd.. 
git clone https://github.com/fogbow/fogbow-client-core.git
cd fogbow-client-core
mvn install -Dmaven.test.skip=true
```

### Use it. <a href="https://github.com/fogbow/fogbowcloud.org/blob/new-cli-documentation/content/pages/fogbow-cli.md">For more information</a>
```
cd fogbow-client-core
bash bin/fogbow-cli ...
```
