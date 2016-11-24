Title: Install and configure Fogbow Dashboard
url: install-configure-fogbow-dashboard
save_as: install-configure-fogbow-dashboard.html
section: install
index: 6

Install and configure the Fogbow Dashboard
==========
The **Fogbow Dashboard** (FD) is a web interface to the **Fogbow Manager** (FM). It provides an easy way to issue all the operations that can be performed through the [Fogbow CLI](http://www.fogbowcloud.org/fogbow-cli).

##Installation

Before installing the FD, you need to install its depencies. In a debian-based distribution, this can be done as follows:

```bash
sudo apt-get install git python-dev python-virtualenv libssl-dev libffi-dev libxml2-dev libxslt1-dev
```

To install the FD, download its lastest stable version from our repository:

``` shell
wget https://github.com/fogbow/fogbow-dashboard/archive/master.zip
```

Then, decompress it:
``` shell
unzip master.zip
```

##Configure
After the installation, assign the proper permissions to the ```keystore``` files

```bash
cd fogbow-dashboard-master
chmod 600 openstack_dashboard/local/.secret_key_store
chmod 600 openstack_dashboard/test/.secret_key_store
```

then, rename the file ```openstack_dashboard/local/local_settings.py.example``` to ```openstack_dashboard/local/local_settings.py.example``` and edit it according to the configuration of your FM.

```bash
# Endpoint Manager
FOGBOW_MANAGER_ENDPOINT = 'http://localhost:8182' # Mandatory

FOGBOW_FEDERATION_AUTH_ENDPOINT = '' # Mandatory
# types : keystone, opennebula, voms, raw_opennebula, raw_keystone, shibboleth, naf, ldap
FOGBOW_FEDERATION_AUTH_TYPE = '' # Mandatory

# for naf federation auth
## dashboard private key path
# FOGBOW_NAF_DASHBOARD_PRIVATE_KEY_PATH='$path_to/dash_private_key.pem'
## dashboard private key path
# FOGBOW_NAF_DASHBOARD_PUBLIC_KEY_PATH='$path_to/dash_public_key.pem'
## portal public key path
# FOGBOW_NAF_PORTAL_PUBLIC_KEY_PATH='$path_to/portal_public_key.pem'

# for ldap federation auth
# PRIVATE_KEY_PATH='$path_to/private_key.pem'
# PUBLIC_KEY_PATH='$path_to/public_key.pem'
# FOGBOW_LDAP_BASE='dc=lsd,dc=ufcg,dc=edu,dc=br'
# FOGBOW_LDAP_ENCRYPT=''

# custom_theme 
# customization of style
CUSTOM_THEME = ''

# Communication between F_dashboard and F_manager
# Default 15 | seconds 
TIMEOUT_POST=15
# Default 15 | seconds
TIMEOUT_DELETE=15
# Default 60 | seconds
TIMEOUT_GET=60
```

After configuring, run the ```./run_tests.sh``` script to download and install the necessary dependencies.
```
bash
./run_tests.sh
```

##Run
To start the FD, run agains the ```run_tests.sh``` script indicating the port that the dashboard will handle requests (```9000```).

``` bash
nohup ./run_tests.sh --runserver localhost:9000 &
```
