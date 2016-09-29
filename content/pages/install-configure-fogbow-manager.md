Title: Install and configure Fogbow Manager
url: install-configure-fogbow-manager
save_as: install-configure-fogbow-manager.html
section: install
index: 3

# Install and configure the Fogbow Manager

The Fogbow Manager is distributed in two forms: as source code, or as a binary package for debian-based distributions. Choose the best distribution for your system, download it and install it as follows.

## Install from source
To get the lastest stable version of the component, download it from our repository:

```bash
wget https://github.com/fogbow/fogbow-manager/archive/master.zip
```

Then, decompress it:

```bash
unzip master.zip
```

Now, install it with Maven:

```bash
cd fogbow-manager
mvn install
```

## Install from debian package

Download a stable version from our <a href="http://downloads.fogbowcloud.org/stable/debian/">package repository</a> and install it with dpkg:

```bash
dpkg -i fogbow-manager_$version.deb
```

## Configure

After the installation, edit three files: ```manager.conf```, ```federation.conf``` and ```infrastructure.conf```. Each file contains proprerties for different aspects of 
Fogbow Manager, like interoperability, cloud infrastructure, managment processes and others. The ```federation.conf``` will define how this installation of Fogbow Manager
interacts with other installations, the rules for the barter of resources and how users will authenticate themselves to use it. The ```manager.conf``` contains properties 
for the FM to control his own execution, like, where to save/load database files, time intervals for monitors execution, configurations about the Fogbow Reverse Tunnel, etc. The ```infrastructure.conf``` has all necessary configurations for FM to work with the cloud infrastructure. At this point there are configurations for Computer Cloud API, Strage Cloud API, Network Cloud API, properties for authentication on the local Cloud, the Image Store functionality and other properties for usage of cloud.

Each properties files will be explained in separate sections.

## Federation properties

### - XMPP

The XMPP properties below indicate the XMMP server that the **Fogbow Manager** (FM) must contact to communicate with its associated **Fogbow Rendezvous** (FR), as well as with other FM in the federation. These properties also indicate the FR associated with the FM.

```bash
# jid of the Fogbow Manager XMPP component
xmpp_jid=my-manager.internal.mydomain

# password of the Fogbow Manager XMPP component
xmpp_password=manager_password

# XMPP server IP address
xmpp_host=IP_of_external.domain

# Port in which the XMPP server will be listening
xmpp_port=5347

# jid of your Rendezvous XMPP component
rendezvous_jid=my-rendezvous.internal.mydomain
```

The ```xmpp_jid```, ```xmpp_password``` and ```rendezvous_jid``` properties above must match the values assigned when the XMPP server was installed (refer to the <a  href="/install-configure-xmpp" target="_blank">Install and configure XMPP </a> section of our documentation).

Following, you need to add a new entry in your DNS to resolve the FM component name to the IP address of the XMPP server like in the example below.

``` shell
my-manager.internal.mydomain        22      IN      A       IP_of_external.domain
```

### - Federation Indentity

This plugin is responsible for control the users access in Fogbow's Management layer. This plugin identifies if the user is authorized to use the federation. The concept of "Federation Member" may change depending on configuration chosen for this plugin. If each federation location (Fogbow Manager installation) has a local authentication method (not centralized), users of this location can't authenticate directly on remote locations, but can request resources there through your Fogbow structure. In this case, all users of one location are seem as an single Federation Member to other federation members. The location is the "Federation Member". However, if all locations (FM) share a common identifying service, like VOMS server or Shiboleth service, each user is an single member of the federation and can request resources directly in any location (FM) of the Federation. The actual plugins for Federation Member Identity are:

- Local identification
	- Keystone Identity Plugin
	- OpenNebula Identity Plugin
	- LDAP
	- X509 Plugin
	- Simple Token Identity Plugin
	- Shiboleth Identity Plugin
	- VOMS
	- NAF

- Shared identification
	- Shiboleth Identity Plugin
	- VOMS
	- NAF	

Each of this plugins has differents propreties to configure.

**Keystone Identity Plugin:**

```
	# Federation Identity plugin class
	federation_identity_class=org.fogbowcloud.manager.core.plugins.openstack.KeystoneIdentityPlugin
	# Federation Identity endpoint
	federation_identity_url=http://$address:$keystone_port

	# Proxy account for remote requests @ the local identity provider
	local_proxy_account_user_name=$user_name
	# Password of such account
	local_proxy_account_password=$password
	# Tenant of such account
	local_proxy_account_tenant_name=$tenant_name
```

**OpenNebula Identity Plugin:**

```
	# Federation Identity plugin class
	federation_identity_class=org.fogbowcloud.manager.core.plugins.opennebula.OpenNebulaIdentityPlugin
	# Federation Identity endpoint
	federation_identity_url=http://$address:port/RPC2

	# Proxy account for remote order @ the local identity provider 
	local_proxy_account_user_name=$user_name
	# Password of such account
	local_proxy_account_password=$user_pass
```

**LDAP:**

LDAP uses PEM files to sign and verify tokens validity. Clientes and Fogbow Manager must use same PEM files for correct identification.
For the LDAP Server informations you should consult your infrastructure administrators.

```
	# Federation Identity plugin class
	federation_identity_class=org.fogbowcloud.manager.core.plugins.identity.ldap.LdapIdentityPlugin
	# Address of LDAP Server.
	ldap_identity_url=ldap://$address:port
	#Example: dc=org,dc=com
	ldap_base=&ldap_base_information
	#encripty type, if ldap implements one.
	ldap_encrypt_type=&encrypt_type

	## Signature informations
	private_key_path=$path_to_private_key_pem_file
	public_key_path=$path_to_public_key_pem_file

```

**X509 Plugin:**

```
	# Federation Identity plugin class
	federation_identity_class=org.fogbowcloud.manager.core.plugins.x509.X509IdentityPlugin

	# Directory where are the certificates of Certificate Authorities (CA). 
	# They are certificates that you trust.
	x509_ca_dir_path=$path_to_ca_directory

```


**Shiboleth Identity Plugin:**

```
	# Federation Identity plugin class
	federation_identity_class=org.fogbowcloud.manager.core.plugins.identity.shibboleth.ShibbolethIdentityPlugin
	# URL for the Shibboleth server.
	identity_shibboleth_get_assertion_url

```

**VOMS:**

```
	# Directory where are the VOMS server information. 
	# List of voms servers in order to issue a proxy. 
	# Default : "~/.glite/vomses"
	path_vomses=$path_vomes

	# Directory where are the certificates of Certificate Authorities (CA). 
	# They are certificates that you trust.
	# You need to have the certificate, CRL (certificate revocation list), 
	# info, namespaces and signing_policy files for each CA.
	# These files need to have read permission grant to the user that runs
	# the fogbow manager
	# Default : "/etc/grid-security/certificates"
	path_trust_anchors=$path_trust_anchors

	# Directory where are the certificates of VOMS that you trust.
	# Default : "/etc/grid-security/vomsdir"
	path_vomsdir=$path_voms_dir

```

**NAF:**

```
	#Only for TokenGenerator
	name_user_token_generator=$user_name_of_token_generetor
	#Only for TokenGenerator
	password_user_token_generator=$password_of_user_of_token_generetor
	#Only for TokenGenerator
	endpoint_token_generator=http://$address:$token_generator_port

	#Must be the public file to the private key .pem file of the TokenGenerator or Dashboard (portal NAF).
	naf_identity_public_key=$path_to_public key_pem_file
```

### - Authorization & Member Validator

+ <h3>Authorization Plugin</h3>

The Authorization Plugin tells whether a given user (with a proper authenticated token) is able to create requests in the Federation. 

#### Configure

The **federation_authorization_class** property must be set to a Authorization Plugin implementation, as shown in the examples below.

**Allow All Authorization Plugin:**

The Allow All pluging simply authorizes any user/token.

```bash
# Federation Authorization plugin class
federation_authorization_class=org.fogbowcloud.manager.core.plugins.authorization.AllowAllAuthorizationPlugin
```
**VO White List Authorization Plugin:**
```bash
# Federation Authorization plugin class
federation_authorization_class=org.fogbowcloud.manager.core.plugins.authorization.voms.VOWhiteListAuthorizationPlugin
authorization_vo_whitelist=$memberOfListOne,$memberOfListTwo,$memberOfListThree
```
**Edu Person White List Authorization Plugin:**
```bash
# Federation Authorization plugin class
federation_authorization_class=org.fogbowcloud.manager.core.plugins.authorization.eduperson.EduPersonWhitelistAuthorizationPlugin
authorization_vo_whitelist=$memberOfListOne,$memberOfListTwo,$memberOfListThree
```
+ <h3>Member Validator Plugin</h3>

The Member Validator plugin determines whether the FM can interact (receive or donate resources) to a given federation member.

#### Configure
The **member_validator_class** property must be set to a Member Authorization Plugin implementation.

**Default Member Authorization Plugin:**

The Default Member Authorization plugin simply authorizes interaction with any federation member.

```bash
# member validator class
member_validator_class=org.fogbowcloud.manager.core.plugins.memberauthorization.DefaultMemberAuthorizationPlugin
```

**VOMS Member Authorization Plugin:**
```bash
# member validator class
member_validator_class=org.fogbowcloud.manager.core.plugins.memberauthorization.VOMSMemberAuthorizationPlugin
member_validator_ca_dir=$path_to_certificate_dir
```

**White List Member Authorization Plugin:**
The White List Member Authorization plugin specifies a list of federations members that the FM can interact with.

```bash
# member validator class
member_validator_class=org.fogbowcloud.manager.core.plugins.memberauthorization.WhitelistMemberAuthorizationPlugin
member_authorization_whitelist_donate_to=$memberOfListOne,$memberOfListTwo,$memberOfListThree
member_authorization_whitelist_receive_from=$memberOfListOne,$memberOfListTwo,$memberOfListThree
```

### - Resource Bartering Control

In Fogbow you could configure how the bartering of resource must works. You can choose how members are selected for resolve
remote requests, configure a Network of Favores (NOF), how to priorize requests and how to account the resourses used for each member.

+ <h3>Member Picker plugin</h3>

The Member Picker plugin is used to choose the federation member that FM will contact to order resources.

#### Configure
Below we show examples for the current supported member picker plugins. The values identified with the $ symbol must be replaced according with the specificities of each deploy.

**Round Robin Member Picker Plugin:**
The Round Robin Member Picker plugin chooses the federation member by alphabetical order in a circular list. 

```bash
# Member picker class
member_picker_class=org.fogbowcloud.manager.core.plugins.memberpicker.RoundRobinMemberPickerPlugin
```

**NOF Member Picker Plugin:**
The Member Picker plugin uses the accounting plugin to choose the federation member. It chooses the federation member witht the biggest debt with the local member
.
```bash
# Member picker class
member_picker_class=org.fogbowcloud.manager.core.plugins.memberpicker.NoFMemberPickerPlugin
```

+ <h3>Capacity Controller Plugin</h3>

The Capacity controller plugin is responsible for calculating a virtual resource quota for each federation member. Its main going is to avoid free riding. It is specially important in scenarios of low resource contention, i.e. when there are exceeding resources and thus the prioritization (plugin) by itself wouldn't be enough to avoid free riding. For more details on this plugin refer to http://www.sciencedirect.com/science/article/pii/S0045790616301082.

#### Configure

Below we show the current available plugins and the parameters we need to configure to tune them.

**Fairness Driven Capacity Controller Plugin:**
```bash
# Capacity Controller class
capacity_controller_class=org.fogbowcloud.manager.core.plugins.capacitycontroller.fairnessdriven.FairnessDrivenCapacityController
```
**Global Fairness Driven Controller Plugin:**

Fairness is a measure of the level of reciprocity to the resources a federation memeber provides, either to another federation member or to the federation as a whole (it is given by the amount of resources consumed divided by the amount of resources donated).

Global Fairness Driven Controller plugin uses exclusively the global fairness (fairness towards the whole federation) in order to decide whether to increase or decrease the amount of resources it should donate to the federation, i.e., the virtual quota. Note that in this implementation a federation member keeps only one single quota to the whole federation.

The configuration of this plugin includes:
* the minimum fairness threshold: a non-negative value indicating the minimum level of fairness desired;
* the maximum fairness threshold: a non-negative value indicating the maximum level of fairness desired (recommended values are *0.75* for the minimum and *0.95* for the maximum, representing that the desired fairness is between 75% and 95%);
* the delta value: the grain used to increase or decrease the virtual quota (recommended values are *0.01* for long term participation and *0.05* for short term participation);
* the maximum capacity: the maximum number of processing instances it is willing to donate.

```bash
# Capacity Controller class
capacity_controller_class=org.fogbowcloud.manager.core.plugins.capacitycontroller.fairnessdriven.GlobalFairnessDrivenController
controller_delta=$delta
controller_minimum_threshold=$min_threshold
controller_maximum_threshold=$max_threshold
controller_maximum_capacity=$max_capacity
```

**Pairwise Fairnesse Driven Controller Plugin:**

Pairwise Fairnesse Driven Controller plugin uses exclusively the pairwise fairness (fairness towards a single member in the federation) in order to decide wether to increase or decrease the virtual quota to the each other member. Note that in this implementation a member keeps multiple quotas, one single quota for each member within the federation. This plugin is configured just like the Global Fairness Driven Controller plugin. 

```bash
# Capacity Controller class
capacity_controller_class=org.fogbowcloud.manager.core.plugins.capacitycontroller.fairnessdriven.PairwiseFairnessDrivenController
controller_delta=$delta
controller_minimum_threshold=$min_threshold
controller_maximum_threshold=$max_threshold
controller_maximum_capacity=$max_capacity
```
**Two Fold Capacity Controller Plugin:**

The Two Fold Capacity Controller plugin reuses the Pairwise and Global approaches to attain better levels of fairness. The global fairness is used basically for newcommers or for members that has only donated without consuming (undefined fairness). The pairwise fairness is used for members that has already consumed any resource.
```bash
# Capacity Controller class
capacity_controller_class=org.fogbowcloud.manager.core.plugins.capacitycontroller.fairnessdriven.TwoFoldCapacityController
```
**Satisfaction Driven Capacity Controller Plugin:**
The Satisfaction Driven Capacity Controller plugin imposes no constraint: it provides all its idle resources to the federation. The more it donates, the higher are the credits it will have with other members, and thus the higher will be its satisfaction. 

```bash
# Capacity Controller class
capacity_controller_class=org.fogbowcloud.manager.core.plugins.capacitycontroller.satisfactiondriven.SatisfactionDrivenCapacityControllerPlugin
```
+ <h3>Prioritization Plugin</h3>

The Prioritization Plugin is responsible for prioritizing a order over another with lower priority. It only comes into play when the quota for creating new resources is exceeded. In these cases, it must verify if in that given time there is any fulfilled/ongoing order from a member with lower priority than the new requester. If this condition is true, that order must preempted so that the new order can be met.

#### Configure

Below we show examples for the available prioritization plugins. The values identified with the $ symbol must be replaced according with the specificities of each deploy.

**Prioritize Remote Order Plugin**
As the name indicates, the Prioritize Remote Order plugin prioritizes the remote orders over local ones.

```bash
# Priorization Plugin class
remote_prioritization_plugin_class=org.fogbowcloud.manager.core.plugins.prioritization.PriotizeRemoteOrderPlugin
```
**Two Fold Prioritization Plugin:**
Two Fold Prioritization plugin allows a more refined prioritization policy by allowing that two other plugins are used in composition. One is used for prioritization of locar orders, and the other for prioritization of remote orders.
```bash
# Priorization Plugin class
remote_prioritization_plugin_class=org.fogbowcloud.manager.core.plugins.prioritization.TwoFoldPrioritizationPlugin
```

**Nof Prioritization Plugin:**
The Nof Prioritization plugin uses the Network of Favors as policy. Roughly, the Network of Favors prioritizes members from which it owes more favors.
```bash
# Priorization Plugin class
remote_prioritization_plugin_class=org.fogbowcloud.manager.core.plugins.prioritization.nof.NoFPrioritizationPlugin
nof_prioritize_local=true
nof_trustworthy=false
```

**FCFS Prioritization Plugin:**
FCFS Prioritization plugin does not perform any prioritization. The first request to come is the first to be served.
```bash
# Priorization Plugin class
local_prioritization_plugin_class=org.fogbowcloud.manager.core.plugins.prioritization.fcfs.FCFSPrioritizationPlugin

```

+ <h3>Accounting Plugin</h3>

The Accounting Plugin is responsible for accounting of the usage of cloud resources such as computing instances and storage. 

#### Configure
The **accounting_class** property must be set to a Accouting Plugin implementation, as shown in the examples below.

**FCU Accounting Plugin:**
The FCU Accounting plugin is based on the total usage time (in minutes) and the power rating determined by benchmarking plugin.

```bash
# Accounting class
compute_accounting_class=org.fogbowcloud.manager.core.plugins.accounting.FCUAccountingPlugin
# Path to accounting database
fcu_accounting_datastore_url=jdbc:sqlite:$path_to_compute_accounting_db
```

**Simple Storage Accounting Plugin:**
The Simples Storage Accounting pluging is based on the total usage time (in minutes) and in the amount of allocated storage.

```bash
# Accounting class
storage_accounting_class=org.fogbowcloud.manager.core.plugins.accounting.SimpleStorageAccountingPlugin
# Path to storage accounting database
simple_storage_accounting_datastore_url=jdbc:sqlite:$path_to_storage_accounting_db
```

Both the above plugins are update according to the **accounting_update_period** below:
```bash
# Time between accounting updates (in milliseconds)
accounting_update_period=300000
```

## Manager properties

### - Databases Properties

The user of FM execution must have permission to read and write on the folders and files informed above.

```
manager_datastore_url=jdbc:sqlite:$path_to_manager_db
instance_datastore_url=jdbc:sqlite:$path_to_instances_db
storage_datastore_url=jdbc:sqlite:$path_to_storage_db
network_datastore_url=jdbc:sqlite:$path_to_network_db
```

### - Time intervals

The FM periodically executes some background tasks, such as resource monitoring. All the time interval properties are defined in the class ```org.fogbowcloud.manager.core.ConfigurationConstants``` and have default period values specified in the class ```org.fogbowcloud.manager.core.ManagerController```. The default values of any of these properties can be overwritten in the configuration file as shown below:

```
#The frequency (in milliseconds) that manager_datastore will be updated with actual orders states.
bd_updater_period=30000

# The scheduler_period property is the time interval (in milliseconds) in which
# the Fogbow Manager submit requests that are not fulfilled yet
scheduler_period=30000
```

### - Fogbow Reverse Tunnel informations

These properties configure the **Fogbow Reverse Tunnel** (FRT) service to provide public IP access to the VMs created by the FM. The example below shows how these properties should be set, considering the example presented in the <a  href="/install-configure-frt" target="_blank">Install and configure Fogbow Reverse Tunnel</a> section of our documentation:

```bash
# The token_host_public_address property defines the public IP address of the FRT service
token_host_public_address=123.456.789.4

# The token_host_private_address property defines the private IP address of the FRT service
token_host_private_address=10.0.0.1

# The token_host_port property defines the port to be used by the VM to establish the SSH tunnel with the FRT
# If this property isn't set, the default value is 2222
token_host_port=2222

```

### - Manager execution properties

```
#IP of this server
my_ip=$manager_ip

# The http_port property indicates http port of the Fogbow Manager service endpoint (API)
http_port=$manager_port

## Benchmarking (Vanilla Benchmarking Plugin) - Plugin for evaluate the process's power of compute resources
benchmarking_class=$benchmarking_class_complete_name

# Benchmarking script to use with SSH Benchmarking plugin - Only if the benchmarking_class is SSHBenchmarkingPlugin
ssh_benchmarking_script_url=http://downloads.fogbowcloud.org/benchmark/script_ssh_benchmarking.sh

## Other properties files - Name of the other files of properties
infrastructure_conf_file=infrastructure.conf
federation_conf_file=federation.conf

```

## Infrastructure properties

### - Image Storage Plugin

The Fogbow orders accepted by the FM contain, among other attributes, the id of the virtual machine image that will execute the order. These ids are federation-wide values and potencially are not recognized at the underlying local cloud. The Image Storage plugin is responsible to translate the image id described in the order and associate it to a valid local image identifier.

+ <h3>VMCatcher Storage Plugin</h3>

The VMCatcher plugin allows users to subscribe to an inventory of virtual machines via the HEPIX image catalog. For more information about vmcatcher, access [vmcatcher page on github.](https://github.com/hepix-virtualisation/vmcatcher)

```bash
## Image Storage Plugin (VMCatcher)
# Image storage class
image_storage_class=org.fogbowcloud.manager.core.plugins.imagestorage.vmcatcher.VMCatcherStoragePlugin
# Run with "sudo"
image_storage_vmcatcher_use_sudo=false
# Path database
image_storage_vmcatcher_env_VMCATCHER_RDBMS="sqlite:////var/lib/vmcatcher/vmcatcher.db"
# Path where stores the images downloaded
image_storage_vmcatcher_env_VMCATCHER_CACHE_DIR_CACHE="/var/lib/vmcatcher/cache"
# Path where stores the images are being download
image_storage_vmcatcher_env_VMCATCHER_CACHE_DIR_DOWNLOAD="/var/lib/vmcatcher/cache/partial"
# Path where stores the images expired
image_storage_vmcatcher_env_VMCATCHER_CACHE_DIR_EXPIRE="/var/lib/vmcatcher/cache/expired"

## Use "glancepush specification" for openstack or "one specification" for opennebula

### Plugin for openstack
### glancepush specific
## Option for use the openstack plugin
# image_storage_vmcatcher_push_method=glancepush
# image_storage_vmcatcher_glancepush_vmcmapping_file=/etc/vmcatcher/vmcmapping
## Path of the plugin
# image_storage_vmcatcher_env_VMCATCHER_CACHE_EVENT="python /var/lib/vmcatcher/gpvcmupdate.py"

### Plugin for opennebula
### one specific
## Option for use the openstack plugin
# image_storage_vmcatcher_push_method=cesga
## Path of the plugin
# image_storage_vmcatcher_env_VMCATCHER_CACHE_EVENT="python /var/lib/vmcatcher/vmcatcher_eventHndl_ON"
## Path where are the user's credentiais
# image_storage_vmcatcher_env_ONE_AUTH="/etc/vmcatcher/one_auth"
```

+ <h3>HTTP Download Image Storage Plugin</h3>

This plugin allows to indicate public accessible URLs that the FM can use to download virtual machines when they are missing in its local cloud.

```bash
image_storage_class=org.fogbowcloud.manager.core.plugins.imagestorage.http.HTTPDownloadImageStoragePlugin

# Base URL of the image repository like the EGI Applications Database
# If the user supplies a relative URL in the request, the base URL will be used to find the image (e.g http://vmappliance-repo.egi.eu/images)
image_storage_http_base_url=http://$vmstore

# Path to the temporary directory where the images should be downloaded to
image_storage_http_tmp_storage=$tmp_path_to_store_vm
```

+ <h3>Static Image Storage</h3>

In addition to the Image Storage plugins is is also possible to statically configure the FM to associate the image federation id to a local identifier, as indicated below.

```bash
image_storage_static_fogbow-linux-x86=$local_id_x86
image_storage_static_fogbow-ubuntu-12.04-with-java=$local_id_javavm
```

Furthermore, the FM also expects that all images configured at manager have **cloud-init** working properly.

### - Cloud-specific plugins

In this section, we show the configuration of the cloud-specific plugins. Theses plugins configure not only the usage of compute, network and storage resources but also how users are identified in each cloud provider. In the examples below, the values identified with the **$** symbol must be replaced according with the specificities of each deploy. The FM distribution contains configuration examples (```manager.conf.[cloudstack,opennebula,openstack,azure].example```) for each cloud orchestrator supported.

**OpenStack:**
```bash
## Compute Plugin
compute_class=org.fogbowcloud.manager.core.plugins.compute.openstack.OpenStackNovaV2ComputePlugin
compute_novav2_url=http://$address:$nova_port
compute_glancev2_url=http://$address:$glance_port
compute_glancev2_image_visibility=private
compute_novav2_network_id=$network_id

## Network Plugin
network_class=org.fogbowcloud.manager.core.plugins.network.openstack.OpenStackV2NetworkPlugin
network_openstack_v2_url=http://$address:$neutron_port
external_gateway_info=$gateway_id

## Storage Plugin
storage_class=org.fogbowcloud.manager.core.plugins.storage.openstack.OpenStackV2StoragePlugin
storage_v2_url=http://$address:$storage_port

## Local Identity
local_identity_class=org.fogbowcloud.manager.core.plugins.identity.openstack.KeystoneIdentityPlugin
local_identity_url=http://$address:$keystone_port

## Local Credentials
federation_user_credentail_class=org.fogbowcloud.manager.core.plugins.localcredentails.SingleMapperPlugin

## Mapper Plugin / Local credentials to be used when we miss information about a given user
mapper_defaults_username=$user_name
mapper_defaults_password=$user_pass
mapper_defaults_tenantname=$tenant_name

## Federation Identity
federation_identity_class=org.fogbowcloud.manager.core.plugins.identity.openstack.KeystoneIdentityPlugin
federation_identity_url=http://$address:$keystone_port
```

**CloudStack:**
```bash
## Compute Plugin
compute_class=org.fogbowcloud.manager.core.plugins.compute.cloudstack.CloudStackComputePlugin
compute_cloudstack_api_url=http://$address/client/api
compute_cloudstack_zone_id=$zone_id
compute_cloudstack_image_download_base_url=http://$address
compute_cloudstack_image_download_base_path=$path_to_download_dir
compute_cloudstack_hypervisor=$hypervisor_type
compute_cloudstack_image_download_os_type_id=$os_type_id
compute_cloudstack_expunge_on_destroy=true
compute_cloudstack_default_networkid=$id_of_default_network

# Network Plugin
network_class=org.fogbowcloud.manager.core.plugins.network.cloudstack.CloudStackNetworkPlugin
network_cloudstack_api_url=https://$address/client/api
network_cloudstack_zone_id=$zone_id
network_cloudstack_netoffering_id=$offering_id

## Storage Plugin
storage_class=org.fogbowcloud.manager.core.plugins.storage.cloudstack.CloudStackStoragePlugin

## Local Identity
local_identity_class=org.fogbowcloud.manager.core.plugins.identity.cloudstack.CloudStackIdentityPlugin
local_identity_url=http://$address/client/api/

## Local Credentials
federation_user_credentail_class=org.fogbowcloud.manager.core.plugins.localcredentails.SingleMapperPlugin

## Mapper Plugin / Local credentials to be used when we miss information about a given user
mapper_defaults_apiKey=$user_api_key
mapper_defaults_secretKey=$user_secret_key
```

**Opennebula:**
```bash
## Compute Plugin
compute_class=org.fogbowcloud.manager.core.plugins.compute.opennebula.OpenNebulaComputePlugin 
compute_one_url=http://$address:$port/RPC2
compute_one_network_id=$network_id
compute_one_datastore_id=$datastore_id
# Below properties allow the FM to copy download VM images to OpenNebula controller machine
# (this is to be used when the FM and the OpenNebula controller run in different machines).
#compute_one_ssh_host=$address
#compute_one_ssh_port=$ssh_port
#compute_one_ssh_username=$user_name
#compute_one_ssh_key_file=$path_to_rsa_key
#compute_one_ssh_target_temp_folder=$path_to_images

# Network plugin
network_class=org.fogbowcloud.manager.core.plugins.network.opennebula.OpenNebulaNetworkPlugin
network_one_bridge=$bridge_id

## Storage Plugin
storage_class=org.fogbowcloud.manager.core.plugins.storage.opennebula.OpenNebulaStoragePlugin
## Default device prefix to use when attaching volumes, values: hd (IDE), sd (SCSI), vd (KVM), vxd (XEN)
storage_one_datastore_default_device_prefix=$prefix

## Local Identity
local_identity_class=org.fogbowcloud.manager.core.plugins.identity.opennebula.OpenNebulaIdentityPlugin
local_identity_url=http://$address:$port/RPC2

## Local Credentials
federation_user_credentail_class=org.fogbowcloud.manager.core.plugins.localcredentails.SingleMapperPlugin

# Mapper Plugin / Local credentials
mapper_defaults_username=$user_name
mapper_defaults_password=$user_pass

## Federation Identity
federation_identity_class=org.fogbowcloud.manager.core.plugins.opennebula.OpenNebulaIdentityPlugin
federation_identity_url=http://$address:$port/RPC2

```

**Azure:**
```bash
## Compute Plugin
compute_class=org.fogbowcloud.manager.core.plugins.compute.azure.AzureComputePlugin
compute_azure_max_instances=$num_max_instances
compute_azure_max_vcpu=$num_max_vcpu
compute_azure_max_ram=$num_max_ram
compute_azure_region=$azure_region

# Network Plugin
network_class=org.fogbowcloud.manager.core.plugins.network.azure.AzureNetworkPlugin

## Storage Plugin
storage_class=org.fogbowcloud.manager.core.plugins.storage.azure.AzureStoragePlugin
compute_azure_storage_account_name=$storage_account_name
compute_azure_storage_key=$key_content

## Identity
federation_identity_class=org.fogbowcloud.manager.core.plugins.identity.azure.AzureIdentityPlugin
mapper_defaults_subscription_id=$subscription_id
mapper_defaults_keystore_path=$path_to_keystore
mapper_defaults_keystore_password=$keystore_pass

## Local Identity
local_identity_class=org.fogbowcloud.manager.core.plugins.identity.azure.AzureIdentityPlugin

```

**Amazon EC2:**
```bash
## Compute Plugin
compute_class=org.fogbowcloud.manager.core.plugins.compute.ec2.EC2ComputePlugin
compute_ec2_region=$ec2_region
compute_ec2_security_group_id=$ec2_secutiry_group_id
compute_ec2_subnet_id=$ec2_subnet_id
compute_ec2_image_bucket_name=$s3_bucket_name
compute_ec2_max_vcpu=$num_max_vcpu
compute_ec2_max_ram=$num_max_ram
compute_ec2_max_instances=$num_max_instances

# Network Plugin
network_class=org.fogbowcloud.manager.core.plugins.network.ec2.EC2NetworkPlugin

## Storage Plugin
storage_class=org.fogbowcloud.manager.core.plugins.storage.ec2.EC2StoragePlugin
storage_ec2_availability_zone=$ec2_storage_availability_zone_id

## Identity
federation_identity_class=org.fogbowcloud.manager.core.plugins.identity.nocloud.NoCloudIdentityPlugin

## Local Identity
local_identity_class=org.fogbowcloud.manager.core.plugins.identity.ec2.EC2IdentityPlugin
mapper_defaults_accessKey=$ec2_access_key
mapper_defaults_secretKey=$ec2_secret_key

```

### - Static Mapping for Flavors (Optional)

If the flavor of the instance is passed as header information, then this properties are used to map the name of the flavor to instance power
(CPU and Memory).

```bash
## Static mapping from flavors to requirements 
flavor_fogbow_small={mem=512, cpu=1}
flavor_fogbow_medium={mem=1024, cpu=2}
flavor_fogbow_large={mem=2048, cpu=4}
```

## Run 
To start the FM, run the ```start-manager``` script inside ```bin```.

```bash
./bin/start-manager
```
