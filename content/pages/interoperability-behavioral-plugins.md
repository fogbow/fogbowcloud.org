Title: Interoperability and behavioral plugins
url: interoperability-behavioral-plugins
save_as: interoperability-behavioral-plugins.html
section: install
index: 7

# Plugins

The **Fogbow Manager** (FM) component has been designed to be agnostic to the underlying cloud technology. This is achieved by the introduction of an interoperability plugin layer between the FM and the cloud. Plugins are instantiated via reflection, and fully configured via the configuration file. Currently, Fogbow makes available some plugins, and new ones can be contributed by the Fogbow developers' community.

In addition to interoperability plugins, there are also behavioral plugins. These are used to specify the way each FM should act when serving client's orders.

## Identity Plugin

The Identity Plugin is responsible for getting and authenticating tokens for local and federation cloud users. The Local Identity Provider and the Federation Identity Provider must be the same when the authentication is done at the Local Cloud Identity Provider. Otherwise, you can configure different plugins for local and federation identity providers. 

Make sure to have the identity plugin ports opened in your network firewall configuration, to allow authentication by federation users from outside the local network, If you are using Keystone Identity Plugin, open the port 5000, or the port 2633 for OpenNebula Identity Plugin.

### Configure

You need to configure the Identity Plugin according to the Identity Provider you are using. The following sections go through the configuration for the Identity Plugins that are currently available. The values identified with the $ symbol must be replaced according with the specificities of each deploy.

#####Keystone Identity Plugin

```bash

# Local Identity plugin class
local_identity_class=org.fogbowcloud.manager.core.plugins.openstack.KeystoneIdentityPlugin
# Cloud Identity endpoint
local_identity_url=http://$address:$keystone_port

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

#####OpenNebula Identity Plugin

```bash

# Local Identity plugin class
local_identity_class=org.fogbowcloud.manager.core.plugins.opennebula.OpenNebulaIdentityPlugin
# Cloud identity endpoint
local_identity_url=http://$address:port/RPC2

# Federation Identity plugin class
federation_identity_class=org.fogbowcloud.manager.core.plugins.opennebula.OpenNebulaIdentityPlugin
# Federation Identity endpoint
federation_identity_url=http://$address:port/RPC2

# Proxy account for remote order @ the local identity provider 
local_proxy_account_user_name=$user_name
# Password of such account
local_proxy_account_password=$user_pass

```

#####X509 Plugin

```bash
# Local Identity plugin class
local_identity_class=org.fogbowcloud.manager.core.plugins.x509.X509IdentityPlugin

# Federation Identity plugin class
federation_identity_class=org.fogbowcloud.manager.core.plugins.x509.X509IdentityPlugin

# Directory where are the certificates of Certificate Authorities (CA). 
# They are certificates that you trust.
x509_ca_dir_path=$path_to_ca_directory
```

#####VOMS Plugin

```bash
# Local Identity plugin class
local_identity_class=org.fogbowcloud.manager.core.plugins.voms.VomsIdentityPlugin

# Federation Identity plugin class
federation_identity_class=org.fogbowcloud.manager.core.plugins.voms.VomsIdentityPlugin

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
Note: The VOMS plugin uses the <a href="https://github.com/italiangrid/voms-api-java" target=_blank>VOMS API Java</a>. This API only works with JREs provided by Oracle with the <a href="http://stackoverflow.com/questions/6481627/java-security-illegal-key-size-or-default-parameters" target=_blank>unlimited strength file installed</a>.

##### No Cloud Identity Plugin
The NoCloud Identity plugin is applied when the FM does not have cloud resources associated with it.
```bash
# Local Identity plugin class
local_identity_class=org.fogbowcloud.manager.core.plugins.identity.nocloud.NoCloudIdentityPlugin
```

##### Simple Token Identity Plugin
```bash
# Federation Identity plugin class
federation_identity_class=org.fogbowcloud.manager.core.plugins.identity.simpletoken.SimpleTokenIdentityPlugin
# Token to check
simple_token_identity_valid_token_id=$token_id
```

##### EC2 Identity Plugin
```bash
# Local Identity plugin class
local_identity_class=org.fogbowcloud.manager.core.plugins.identity.ec2.EC2IdentityPlugin

# Federation Identity plugin class
federation_identity_class=org.fogbowcloud.manager.core.plugins.identity.ec2.EC2IdentityPlugin
```
##### Azure Identity Plugin
```bash
# Local Identity plugin class
local_identity_class=org.fogbowcloud.manager.core.plugins.identity.azure.AzureIdentityPlugin

# Federation Identity plugin class
federation_identity_class=org.fogbowcloud.manager.core.plugins.identity.azure.AzureIdentityPlugin
```
##### CloudStack Identity Plugin
```bash
# Federation Identity plugin class
federation_identity_class=org.fogbowcloud.manager.core.plugins.identity.cloudstack.CloudStackIdentityPlugin
# Federation Identity endpoint
federation_identity_url=http://$address:$port/client/api/

# Local Identity plugin class
local_identity_class=org.fogbowcloud.manager.core.plugins.identity.cloudstack.CloudStackIdentityPlugin
# Cloud Identity endpoint
local_identity_url=http://$address:$port/client/api/
```
##### Shiboleth Identity Plugin
```bash
# Federation Identity plugin class
federation_identity_class=org.fogbowcloud.manager.core.plugins.identity.shibboleth.ShibbolethIdentityPlugin
```

## Compute Plugin

The Compute Plugin is responsible for requesting, getting, and deleting instances in the local cloud.

### Configure

Different plugins can require different information depending on their implementation. The values identified with the $ symbol must be replaced according with the specificities of each deploy. The examples below show the required properties for the plugins currently available. 

##### OpenStack OCCI Compute Plugin

```bash
# Compute plugin class
compute_class=org.fogbowcloud.manager.core.plugins.openstack.OpenStackComputePlugin

# Cloud OCCI endpoint
compute_occi_url=http://$address:$v2_port

# Cloud v2 compute endpoint
compute_openstack_v2api_url=http://$address:$v2_port

# Associating local cloud flavors to fogbow flavors
# Small flavor
compute_occi_flavor_small=m1-small

# Medium flavor
compute_occi_flavor_medium=m1-medium

# Large flavor
compute_occi_flavor_large=m1-large

# OS Cloud Scheme
compute_occi_os_scheme=http://schemas.openstack.org/template/os#

# Instance Cloud Scheme
compute_occi_instance_scheme=http://schemas.openstack.org/compute/instance#

# Resource Cloud Scheme
compute_occi_resource_scheme=http://schemas.openstack.org/template/resource#

# Network ID (This property is required only if user project has more 
# than one network available)
# Example:
compute_occi_network_id=$network_id
```

##### OpenStack Nova V2 Compute Plugin
```bash
# Plugin class
compute_class=org.fogbowcloud.manager.core.plugins.compute.openstack.OpenStackNovaV2ComputePlugin
# Nova V2 API URL
compute_novav2_url=http://$address:$nova_port

# Image Store Service
compute_glancev2_url=http://$address:$glance_port
compute_glancev2_image_visibility=private

# Network id (This property is required only if user project has more 
# than one network available)
compute_novav2_network_id=$network_id
```

##### OpenNebula Compute Plugin

```bash
# Compute plugin class
compute_class=org.fogbowcloud.manager.core.plugins.opennebula.OpenNebulaComputePlugin

# Cloud opennebula compute endpoint
compute_one_url=http://$addess:$port/RPC2

# Associating properties flavors to fogbow flavors
# Small flavor
compute_one_flavor_small={mem=128, cpu=1}

# Medium flavor
compute_one_flavor_medium={mem=256, cpu=2}

# Large flavor
compute_one_flavor_large={mem=512, cpu=4}

# Network ID to be used for instances
compute_one_network_id=$network_id

# Settings used by ONE compute plugin to register new images in the cloud
# ID of datastore to register the image
compute_one_datastore_id=$datastore_id
# To register a new image, the image file needs to be in the some machine where ONE is running
# If the fogbow manager is running in a different machine, set the SSH properties to transfer the image
# Or if the fogbow manager is in the same machine, leave it blank
compute_one_ssh_host=$ssh_address
compute_one_ssh_port=$ssh_port
compute_one_ssh_username=$user_name
# The SSH try to access using private key, set the path to ssh id_rsa file
compute_one_ssh_key_file=$path_to_rsa_key
# Set the directory to copy images in remote host
compute_one_ssh_target_temp_folder=$path_to_images
```
##### No Cloud Compute Plugin
The NoCloud Compute plugin is applied when the FM does not have cloud resources associated with it.
```bash
# Compute plugin class
compute_class=org.fogbowcloud.manager.core.plugins.compute.nocloud.NoCloudComputePlugin
```
##### EC2 Compute Plugin
```bash
# Compute plugin class
compute_class=org.fogbowcloud.manager.core.plugins.compute.ec2.EC2ComputePlugin
# Region where will create the VM
compute_ec2_region=$ec2_region
# Security group id given in the user's account
compute_ec2_security_group_id=$ec2_secutiry_group_id
# Subnet id given in the user's account
compute_ec2_subnet_id=$ec2_subnet_id
compute_ec2_image_bucket_name=$s3_bucket_name
# amount maximum of vCPU
compute_ec2_max_vcpu=$num_max_vcpu
# amount maximum of RAM
compute_ec2_max_ram=$num_max_ram
# amount maximum of instances
compute_ec2_max_instances=$num_max_instances
```

##### Azure Compute Plugin
```bash
# Compute plugin class
compute_class=org.fogbowcloud.manager.core.plugins.compute.azure.AzureComputePlugin
# Limit of vCPUs to use
compute_azure_max_instances=$num_max_instances
# Limit of memory to use
compute_azure_max_ram=$num_max_ram
# Region in Azure to create VMs and resources
compute_azure_region=$azure_region
``` 

##### CloudStack Compute Plugin
```bash
## Compute Plugin
compute_class=org.fogbowcloud.manager.core.plugins.compute.cloudstack.CloudStackComputePlugin
# URL of CloudStack API
compute_cloudstack_api_url=http://$address/client/api
# ID of the CloudStack zone to create VMs
compute_cloudstack_zone_id=$zone_id
# Base URL of the webserver where your image repository is configured
compute_cloudstack_image_download_base_url=http://$address
# Absolute path of your image repository in the webserver
compute_cloudstack_image_download_base_path=$path_to_download_dir
# Hypervisor type used in CloudStack
compute_cloudstack_hypervisor=$hypervisor_type
# ID of the default OS type to register new images
compute_cloudstack_image_download_os_type_id=$os_type_id
# Defines if the VM should be completely removed on terminate
compute_cloudstack_expunge_on_destroy=true
``` 

## Image Storage Plugin

The Fogbow orders accepted by the FM contain, among other attributes, the id of the virtual machine image that will execute the order. These ids are federation-wide values and potencially are not recognized at the underlying local cloud. The Image Storage plugin is responsible to translate the image id described in the order and associate it to a valid local image identifier.

``*Important: `` All images must support [cloud-init](https://cloudinit.readthedocs.io/en/latest/).

##### VMCatcher Storage Plugin

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

##### HTTP Download Image Storage Plugin
This plugin allows to indicate public accessible URLs that the FM can use to download virtual machines when they are missing in its local cloud.

```bash
image_storage_class=org.fogbowcloud.manager.core.plugins.imagestorage.http.HTTPDownloadImageStoragePlugin

# Base URL of the image repository like the EGI Applications Database
# If the user supplies a relative URL in the request, the base URL will be used to find the image (e.g http://vmappliance-repo.egi.eu/images)
image_storage_http_base_url=http://$vmstore

# Path to the temporary directory where the images should be downloaded to
image_storage_http_tmp_storage=$tmp_path_to_store_vm
```

##### Static Image Storage

In addition to the Image Storage plugins is is also possible to statically configure the FM to associate the image federation id to a local identifier, as indicated below.

```bash
image_storage_static_fogbow-linux-x86=$local_id_x86
image_storage_static_fogbow-ubuntu-12.04-with-java=$local_id_javavm
```

Furthermore, the FM also expects that all images configured at manager have **cloud-init** working properly.

## Authorization Plugin

The Authorization Plugin tells whether a given user (with a proper authenticated token) is able to create requests in the Federation. 

### Configure

The **federation_authorization_class** property must be set to a Authorization Plugin implementation, as shown in the examples below.

##### Allow All Authorization Plugin

The Allow All pluging simply authorizes any user/token.

```bash
# Federation Authorization plugin class
federation_authorization_class=org.fogbowcloud.manager.core.plugins.authorization.AllowAllAuthorizationPlugin
```
##### VO White List Authorization Plugin
```bash
# Federation Authorization plugin class
federation_authorization_class=org.fogbowcloud.manager.core.plugins.authorization.voms.VOWhiteListAuthorizationPlugin
authorization_vo_whitelist=$memberOfListOne,$memberOfListTwo,$memberOfListThree
```
##### Edu Person White List Authorization Plugin
```bash
# Federation Authorization plugin class
federation_authorization_class=org.fogbowcloud.manager.core.plugins.authorization.eduperson.EduPersonWhitelistAuthorizationPlugin
authorization_vo_whitelist=$memberOfListOne,$memberOfListTwo,$memberOfListThree
```

## Member Authorization Plugin

The Member Authorization plugin determines whether the FM can interact (receive or donate resources) to a given federation member.

### Configure
The **member_validator_class** property must be set to a Member Authorization Plugin implementation.

##### Default Member Authorization Plugin

The Default Member Authorization plugin simply authorizes interaction with any federation member.

```bash
# member validator class
member_validator_class=org.fogbowcloud.manager.core.plugins.memberauthorization.DefaultMemberAuthorizationPlugin
```

##### VOMS Member Authorization Plugin
```bash
# member validator class
member_validator_class=org.fogbowcloud.manager.core.plugins.memberauthorization.VOMSMemberAuthorizationPlugin
member_validator_ca_dir=$path_to_certificate_dir
```

##### White List Member Authorization Plugin
The White List Member Authorization plugin specifies a list of federations members that the FM can interact with.

```bash
# member validator class
member_validator_class=org.fogbowcloud.manager.core.plugins.memberauthorization.WhitelistMemberAuthorizationPlugin
member_authorization_whitelist_donate_to=$memberOfListOne,$memberOfListTwo,$memberOfListThree
member_authorization_whitelist_receive_from=$memberOfListOne,$memberOfListTwo,$memberOfListThree
```

## Storage Plugin
The Storage Plugin is responsible for requesting, getting, and deleting storage resouces in the local cloud.

### Configure

Different plugins require different information depending on their implementation. Below we show examples for the current supported clouds providers. The values identified with the $ symbol must be replaced according with the specificities of each deploy.

##### OpenStack V2 Storage Plugin
```bash
# Storage Plugin class
storage_class=org.fogbowcloud.manager.core.plugins.storage.openstack.OpenStackV2StoragePlugin
storage_v2_url=http://$address:$storage_port

```
##### Opennebula Storage Plugin
```bash
# Storage Plugin class
storage_class=org.fogbowcloud.manager.core.plugins.storage.opennebula.OpenNebulaStoragePlugin
## Default device prefix to use when attaching volumes, values: hd (IDE), sd (SCSI), vd (KVM), vxd (XEN)
storage_one_datastore_default_device_prefix=$prefix
```
##### No Cloud Storage Plugin
The NoCloud Compute plugin is applied when the FM does not have cloud resources associated with it.
```bash
# Storage Plugin class
storage_class=orgorg.fogbowcloud.manager.core.plugins.storage.nocloud.NoCloudStoragePlugin
```
##### EC2 Storage Plugin
```bash
# Storage Plugin class
storage_class=org.fogbowcloud.manager.core.plugins.storage.ec2.EC2StoragePlugin
storage_ec2_availability_zone=$ec2_storage_availability_zone_id
```
##### Azure Storage Plugin
```bash
# Storage Plugin class
storage_class=org.fogbowcloud.manager.core.plugins.storage.azure.AzureStoragePlugin
# Name of the storage account to create the VM storage disk
compute_azure_storage_account_name=$storage_account_name
# Access key of the configured storage account
compute_azure_storage_key=$key_content
```
##### CloudStack Storage Plugin
```bash
# Storage Plugin class
storage_class=org.fogbowcloud.manager.core.plugins.storage.cloudstack.CloudStackStoragePlugin
```

## Network Plugin
The Network Plugin is responsible for requesting, getting, and deleting networking resources in the local cloud. Different plugins require different information depending on their implementation. Below we show examples for the current supported clouds providers. The values identified with the $ symbol must be replaced according with the specificities of each deploy.

### Configure
##### OpenStack V2 Network Plugin
```bash
# Network Plugin class
network_class=org.fogbowcloud.manager.core.plugins.network.openstack.OpenStackV2NetworkPlugin
# URL of OpenStack networking API
network_openstack_v2_url=http://$address:$neutron_port
# ID of the networking gateway that will be associated with the networking resource
external_gateway_info=$gateway_id
```
##### Opennebula Network Plugin
```bash
# Network Plugin class
network_class=org.fogbowcloud.manager.core.plugins.network.opennebula.OpenNebulaNetworkPlugin
# Id of the Opennenula bridge
network_one_bridge=$bridge_id

```
##### No Cloud Network Plugin
The NoCloud Network plugin is applied when the FM does not have cloud resources associated with it.
```bash
# Network Plugin class
network_class=org.fogbowcloud.manager.core.plugins.network.nocloud.NoCloudNetworkPlugin
```
##### EC2 Cloud Network Plugin
```bash 
# Network Plugin class
network_class=org.fogbowcloud.manager.core.plugins.network.ec2.EC2NetworkPlugin
```
##### CloudStack Network Plugin
```bash 
# Network Plugin class
network_class=org.fogbowcloud.manager.core.plugins.network.cloudstack.CloudStackNetworkPlugin
# URL of CloudStack API
network_cloudstack_api_url=https://$address/client/api
# ID of the CloudStack zone to create networking resources
network_cloudstack_zone_id=$zone_id
# ID of the template used to create networking resources
network_cloudstack_netoffering_id=$offering_id
```

##### AWS Network Plugin
```bash
# Network Plugin class
network_class=org.fogbowcloud.manager.core.plugins.network.ec2.EC2NetworkPlugin
```
##### Azure Network Plugin
```bash
# Network Plugin class
network_class=org.fogbowcloud.manager.core.plugins.network.azure.AzureNetworkPlugin
```

## Accounting Plugin
The Accounting Plugin is responsible for accounting of the usage of cloud resources such as computing instances and storage. 
### Configure
The **accounting_class** property must be set to a Accouting Plugin implementation, as shown in the examples below.

##### FCU Accounting Plugin
The FCU Accounting plugin is based on the total usage time (in minutes) and the power rating determined by benchmarking plugin.

```bash
# Accounting class
compute_accounting_class=org.fogbowcloud.manager.core.plugins.accounting.FCUAccountingPlugin
# Path to accounting database
fcu_accounting_datastore_url=jdbc:sqlite:$path_to_compute_accounting_db
```

##### Simple Storage Accounting Plugin
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

## Benchmarking Plugin
The Benchmarking pluging is used to calculate the power rating of a virtual machine.

### Configure

Below we show examples for the current supported benchmarking plugins. The values identified with the $ symbol must be replaced according with the specificities of each deploy.

##### SSH Benchmarking Plugin

SSH Benchmarking plugin calculates the power rating based on the time to execute a bechmark code.

```bash
# Benchmarking class
benchmarking_class=org.fogbowcloud.manager.core.plugins.benchmarking.SSHBenchmarkingPlugin
# Benchmarking script to use with SSH Benchmarking plugin
ssh_benchmarking_script_url=http://downloads.fogbowcloud.org/benchmark/script_ssh_benchmarking.sh
# FM public and private keys
ssh_private_key=$path_to_private_key
ssh_public_key=$path_to_public_key
```
##### Vanilla Benchmarking Plugin

The Vanilla Benchmarking plugin calculates the power rating based on the capabilities of the virtual machine instace such as VCPU and memory.

```bash
# Benchmarking class
benchmarking_class=org.fogbowcloud.manager.core.plugins.benchmarking.VanillaBenchmarkingPlugin
```

## Member Picker Plugin
The Member Picker plugin is used to choose the federation member that FM will contact to order resources.

### Configure
Below we show examples for the current supported member picker plugins. The values identified with the $ symbol must be replaced according with the specificities of each deploy.

##### Round Robin Member Picker Plugin
The Round Robin Member Picker plugin chooses the federation member by alphabetical order in a circular list. 

```bash
# Member picker class
member_picker_class=org.fogbowcloud.manager.core.plugins.memberpicker.RoundRobinMemberPickerPlugin
```

##### NOF Member Picker Plugin
The Member Picker plugin uses the accounting plugin to choose the federation member. It chooses the federation member witht the biggest debt with the local member
.
```bash
# Member picker class
member_picker_class=org.fogbowcloud.manager.core.plugins.memberpicker.NoFMemberPickerPlugin
```

## Capacity controller plugin

The Capacity controller plugin is responsible for calculating a virtual resource quota for each federation member. Its main going is to avoid free riding. It is specially important in scenarios of low resource contention, i.e. when there are exceeding resources and thus the prioritization (plugin) by itself wouldn't be enough to avoid free riding. For more details on this plugin refer to http://www.sciencedirect.com/science/article/pii/S0045790616301082.

### Configure

Below we show the current available plugins and the parameters we need to configure to tune them.

##### Fairness Driven Capacity Controller Plugin
```bash
# Capacity Controller class
capacity_controller_class=org.fogbowcloud.manager.core.plugins.capacitycontroller.fairnessdriven.FairnessDrivenCapacityController
```
##### Global Fairness Driven Controller Plugin

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

##### Pairwise Fairnesse Driven Controller Plugin

Pairwise Fairnesse Driven Controller plugin uses exclusively the pairwise fairness (fairness towards a single member in the federation) in order to decide wether to increase or decrease the virtual quota to the each other member. Note that in this implementation a member keeps multiple quotas, one single quota for each member within the federation. This plugin is configured just like the Global Fairness Driven Controller plugin. 

```bash
# Capacity Controller class
capacity_controller_class=org.fogbowcloud.manager.core.plugins.capacitycontroller.fairnessdriven.PairwiseFairnessDrivenController
controller_delta=$delta
controller_minimum_threshold=$min_threshold
controller_maximum_threshold=$max_threshold
controller_maximum_capacity=$max_capacity
```
##### Two Fold Capacity Controller Plugin

The Two Fold Capacity Controller plugin reuses the Pairwise and Global approaches to attain better levels of fairness. The global fairness is used basically for newcommers or for members that has only donated without consuming (undefined fairness). The pairwise fairness is used for members that has already consumed any resource.
```bash
# Capacity Controller class
capacity_controller_class=org.fogbowcloud.manager.core.plugins.capacitycontroller.fairnessdriven.TwoFoldCapacityController
```
##### Satisfaction Driven Capacity Controller Plugin
The Satisfaction Driven Capacity Controller plugin imposes no constraint: it provides all its idle resources to the federation. The more it donates, the higher are the credits it will have with other members, and thus the higher will be its satisfaction. 

```bash
# Capacity Controller class
capacity_controller_class=org.fogbowcloud.manager.core.plugins.capacitycontroller.satisfactiondriven.SatisfactionDrivenCapacityControllerPlugin
```

## Mapper Plugin
The Mapper plugin determines the policy used to map a federation user into an user in the local cloud. 

### Configure
The mapping is defined based on a *identificator* and on the specific credential of each local identity plugin. It uses the sintax *mapper_ + {identificator} + _ + {credential}* to specify the mapping. Below, we show examples for the current available plugins:

```bash
# Openstack V2 credentials: username, password, tenantName
# Identificator: defaults
mapper_defaults_username=$user_name
mapper_defaults_password=$user_pass
mapper_defaults_tenantName=$tenant_name
# Identificator: other
# mapper_other_username=$other_user_name
# mapper_other_password=$other_user_pass
# mapper_other_tenantName=$other_tenant_name

# Openstack V3 credentials: userId, password, projectId
# Identificator: defaults
mapper_defaults_userId=$user_id
mapper_defaults_password=$user_pass
mapper_defaults_projectId=$project_id

# Opennebula credentials: username, password
# Identificator: defaults
mapper_defaults_username=$user_name
mapper_defaults_password=$user_pass

# EC2 credentials: accessKey, secretKey
# Identificator: defaults
mapper_defaults_accessKey=$access_key
mapper_defaults_secretKey=$secret_key

# Azure credentials: subscription_id, keystore_path, keystore_password
# Identificator: defaults
mapper_defaults_subscription_id=$subscription_id
mapper_defaults_keystore_path=$keystore_path
mapper_defaults_keystore_password=$keystore_pass

## CloudStack credentials: apiKey, secretKey
mapper_defaults_apiKey=$user_api_key
mapper_defaults_secretKey=$user_secret_key

```

##### Federation User Based Mapper Plugin
In the Federation User Based Mapper plugin the user name present in the token is used as *identificator*.

```bash
# Mapper class
federation_user_credentail_class=org.fogbowcloud.manager.core.plugins.localcredentails.FederationUserBasedMapperPlugin

# Example 
# Identificator: tokenuser
mapper_tokenuser_username=$user_name
mapper_tokenuser_password=$user_pass
mapper_tokenuser_tenantName=$tenant_name

# defaults
mapper_defaults_username=$default_user_name
mapper_defaults_password=$default_user_pass
mapper_defaults_tenantName=$default_tenant_name
```
##### Member Based Mapper Plugin
In the Member Based Mapper plugin, the federation member is used as *identificator*.

```bash
# Mapper class
federation_user_credentail_class=org.fogbowcloud.manager.core.plugins.localcredentails.MemberBasedMapperPlugin

# Example 
# Identificator: manager.com.br
mapper_manager.com.br_username=$user_name
mapper_manager.com.br_password=$user_pass
mapper_manager.com.br_tenantName=$tenant_name

# defaults
mapper_defaults_username=$default_user_name
mapper_defaults_password=$default_user_pass
mapper_defaults_tenantName=$default_tenant_name
```
##### Simple Mapper Plugin
In Simple Mapper plugin, only the default identifier is applied.

```bash
# Mapper class
federation_user_credentail_class=org.fogbowcloud.manager.core.plugins.localcredentails.SingleMapperPlugin

# Example 
# Identificator: defaults
mapper_defaults_username=$default_user_name
mapper_defaults_defaults_password=$default_user_pass
mapper_defaults_tenantName=$default_tenant_name
```

##### VOBased Mapper Plugin
In the VOBased Mapper Plugin, the *CN* of the VOMS token is used as identifier.

```bash
# Mapper class
federation_user_credentail_class=org.fogbowcloud.manager.core.plugins.localcredentails.VOBasedMapperPlugin

# Example 
# Identificador: CN->Fulano,OU->DSC,O->UFCG,O->UFFBrGridCA,O->ICPEDU,C->BR
mapper_CN->Fulano,OU->DSC,O->UFCG,O->UFFBrGridCA,O->ICPEDU,C->BR_username=$user_name
mapper_CN->Fulano,OU->DSC,O->UFCG,O->UFFBrGridCA,O->ICPEDU,C->BR_password=$user_pass
mapper_CN->Fulano,OU->DSC,O->UFCG,O->UFFBrGridCA,O->ICPEDU,C->BR_tenantName=$tenant_name

# defaults
mapper_defaults_username=$default_user_name
mapper_defaults_defaults_password=$default_user_pass
mapper_defaults_tenantName=$default_tenant_name
```

## Prioritization Plugin

The Prioritization Plugin is responsible for prioritizing a order over another with lower priority. It only comes into play when the quota for creating new resources is exceeded. In these cases, it must verify if in that given time there is any fulfilled/ongoing order from a member with lower priority than the new requester. If this condition is true, that order must preempted so that the new order can be met.

### Configure

Below we show examples for the available prioritization plugins. The values identified with the $ symbol must be replaced according with the specificities of each deploy.

##### Prioritize Remote Order Plugin
As the name indicates, the Prioritize Remote Order plugin prioritizes the remote orders over local ones.

```bash
# Priorization Plugin class
remote_prioritization_plugin_class=org.fogbowcloud.manager.core.plugins.prioritization.PriotizeRemoteOrderPlugin
```
##### Two Fold Prioritization Plugin
Two Fold Prioritization plugin allows a more refined prioritization policy by allowing that two other plugins are used in composition. One is used for prioritization of locar orders, and the other for prioritization of remote orders.
```bash
# Priorization Plugin class
remote_prioritization_plugin_class=org.fogbowcloud.manager.core.plugins.prioritization.TwoFoldPrioritizationPlugin
```

##### Nof Prioritization Plugin
The Nof Prioritization plugin uses the Network of Favors as policy. Roughly, the Network of Favors prioritizes members from which it owes more favors.
```bash
# Priorization Plugin class
remote_prioritization_plugin_class=org.fogbowcloud.manager.core.plugins.prioritization.nof.NoFPrioritizationPlugin
nof_prioritize_local=true
nof_trustworthy=false
```

##### FCFS Prioritization Plugin
FCFS Prioritization plugin does not perform any prioritization. The first request to come is the first to be served.
```bash
# Priorization Plugin class
local_prioritization_plugin_class=org.fogbowcloud.manager.core.plugins.prioritization.fcfs.FCFSPrioritizationPlugin

```
