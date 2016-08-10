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
# Name of the storage account to create the VM storage disk
compute_azure_storage_account_name=$storage_account_name
# Access key of the configured storage account
compute_azure_storage_key=$key_content
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
The Storage Plugin is responsible for requesting, getting, and deleting storage at the local cloud. Different plugins can require different information depending on their implementation. Fogbow manager assumes that all cloud users have quota defined and all information at ```manager.conf``` file are correct. If not, the behaviour of federation may not be the expected.

### Configure

##### OpenStack V2 Storage Plugin
```bash
# Storage Plugin class
storage_class=org.fogbowcloud.manager.core.plugins.storage.openstack.OpenStackV2StoragePlugin
storage_v2_url=http://localhost:8776
```
##### Opennebula Storage Plugin
```bash
# Storage Plugin class
storage_class=org.fogbowcloud.manager.core.plugins.storage.opennebula.OpenNebulaStoragePlugin
# Default device prefix to use when attaching volumes, values: hd (IDE), sd (SCSI), vd (KVM), vxd (XEN)
storage_one_datastore_default_device_prefix=vd
```
##### No Cloud Storage Plugin
Cloud Storage Plugin describe a scenary that does not exist an cloud  that is associate to a Fogbow manager.
```bash
# Storage Plugin class
storage_class=org.fogbowcloud.manager.core.plugins.storage.cloudstack.CloudStackStoragePlugin
```
##### EC2 Storage Plugin
```bash
# Storage Plugin class
storage_class=org.fogbowcloud.manager.core.plugins.storage.ec2.EC2StoragePlugin
storage_ec2_availability_zone=us-east-1b
```
##### Azure Storage Plugin
```bash
# Storage Plugin class
storage_class=org.fogbowcloud.manager.core.plugins.storage.azure.AzureStoragePlugin
compute_azure_storage_account_name=
compute_azure_storage_key=
```
##### CloudStack Storage Plugin
```bash
# Storage Plugin class
storage_class=org.fogbowcloud.manager.core.plugins.storage.cloudstack.CloudStackStoragePlugin
```

## Network Plugin
The Network Plugin is responsible for requesting, getting, and deleting network at the local cloud. Different plugins can require different information depending on their implementation. Fogbow manager assumes that all cloud users have quota defined and all information at ```manager.conf``` file are correct. If not, the behaviour of federation may not be the expected.

### Configure
##### OpenStack V2 Network Plugin
```bash
# Network Plugin class
network_class=org.fogbowcloud.manager.core.plugins.network.openstack.OpenStackV2NetworkPlugin
network_openstack_v2_url=http://localhost:9696
external_gateway_info=ea51ed0c-0e8a-448d-8202-c79777109ffe
```
##### Opennebula Network Plugin
```bash
# Network Plugin class
network_class=org.fogbowcloud.manager.core.plugins.network.opennebula.OpenNebulaNetworkPlugin
network_one_bridge=br0
```
##### No Cloud Network Plugin
Cloud Compute Plugin describe a scenary that does not exist an cloud  that is associate to a Fogbow manager.

```bash
# Network Plugin class
network_class=org.fogbowcloud.manager.core.plugins.network.nocloud.NoCloudNetworkPlugin
```
##### EC2 Cloud Network Plugin
```bash 
# Network Plugin class
network_class=org.fogbowcloud.manager.core.plugins.network.ec2.EC2NetworkPlugin
network_ec2_region=
```
##### CloudStack Network Plugin
```bash 
network_class=org.fogbowcloud.manager.core.plugins.network.cloudstack.CloudStackNetworkPlugin
network_cloudstack_api_url=https://$address/client/api
network_cloudstack_zone_id=$zone_id
network_cloudstack_netoffering_id=$offering_id
```


## Accounting Plugin
The Accounting Plugin is responsible for accounting of the instances and storages. 
### Configure
Configuração comum: 
```bash
# Periodo de atualização do accouting em milisegundos
accounting_update_period=300000
```

##### FCU Accounting Plugin
The plugin calculate from sum of the use minutes and the power rating determined by benchmarking plugin.

```bash
# Accounting class
accounting_class=org.fogbowcloud.manager.core.plugins.accounting.FCUAccountingPlugin
# Path database
fcu_accounting_datastore_url=jdbc:sqlite:/tmp/computeusage
```

##### Simple Storage Accounting Plugin
The plugin calculate from sum of the use minutes and the storage size.

```bash
# Accounting class
storage_accounting_class=org.fogbowcloud.manager.core.plugins.accounting.SimpleStorageAccountingPlugin
# Path database
simple_storage_accounting_datastore_url=jdbc:sqlite:/tmp/storageusage
```

## Benchmarking Plugin
Benchmarking used to calculate the power rating of the VM. 
### Configure
##### SSH Benchmarking Plugin
The calculation of instance power rating is done from execution time of the script executed in the instance per SSH.

```bash
# Benchmarking class
benchmarking_class=org.fogbowcloud.manager.core.plugins.benchmarking.SSHBenchmarkingPlugin
# Benchmarking script to use with SSH Benchmarking plugin
ssh_benchmarking_script_url=http://downloads.fogbowcloud.org/benchmark/script_ssh_benchmarking.sh
# Manager public and private keys
ssh_private_key=/etc/fogbow-manager/ssh/id_rsa
ssh_public_key=/etc/fogbow-manager/ssh/id_rsa.pub
```
##### Vanilla Benchmarking Plugin
The calculation of instance power rating is done from capabilities of the instance. This cababilities are VCPU and memorie.

```bash
# Benchmarking class
benchmarking_class=org.fogbowcloud.manager.core.plugins.benchmarking.VanillaBenchmarkingPlugin
```

## Member Picker Plugin
Choice of a federation member that Fogbow Manager will order for resource.

### Configure
##### Round Robin Member Picker Plugin
The plugin choose the federation member by alphabetical order in a list.

```bash
# Member picker class
member_picker_class=org.fogbowcloud.manager.core.plugins.memberpicker.RoundRobinMemberPickerPlugin
```

##### NOF Member Picker Plugin
The plugin use the accounting plugin for decide the choice of the federation member. This choice is determined based on the debit of the federatio member with the local member. When biggest the debit, more propitious is be chosen.
```bash
# Member picker class
member_picker_class=org.fogbowcloud.manager.core.plugins.memberpicker.NoFMemberPickerPlugin
```

## Capacity controller plugin
...

### Configure
##### Fairness Driven Capacity Controller Plugin
```bash
# Capacity Controller class
capacity_controller_class=org.fogbowcloud.manager.core.plugins.capacitycontroller.fairnessdriven.FairnessDrivenCapacityController
```
##### Global Fairness Driven Controller Plugin
```bash
# Capacity Controller class
capacity_controller_class=org.fogbowcloud.manager.core.plugins.capacitycontroller.fairnessdriven.GlobalFairnessDrivenController
controller_delta=
controller_minimum_threshold=
controller_maximum_threshold=
controller_maximum_capacity=
```
##### Hill Climbing Algorithm Plugin
```bash
# Capacity Controller class
capacity_controller_class=org.fogbowcloud.manager.core.plugins.capacitycontroller.fairnessdriven.HillClimbingAlgorithm
```
##### Pairwise Fairnesse Driven Controller Plugin
```bash
# Capacity Controller class
capacity_controller_class=org.fogbowcloud.manager.core.plugins.capacitycontroller.fairnessdriven.PairwiseFairnessDrivenController
controller_delta=
controller_minimum_threshold=
controller_maximum_threshold=
controller_maximum_capacity=
```
##### Two Fold Capacity Controller Plugin
```bash
# Capacity Controller class
capacity_controller_class=org.fogbowcloud.manager.core.plugins.capacitycontroller.fairnessdriven.TwoFoldCapacityController
```
##### Satisfaction Driven Capacity Controller Plugin
```bash
# Capacity Controller class
capacity_controller_class=org.fogbowcloud.manager.core.plugins.capacitycontroller.satisfactiondriven.SatisfactionDrivenCapacityControllerPlugin
```

## Mapper Plugin
Policy to map a federation user in one determinate user in the cloud. 
### Configure
This mapping is done with a identificator, that will be determinated per one of plugins, and the credentials referent to local identity plugin. When not possible to find a identificator per plugin is used a identificator default mandatorily.

Formulate : 
mapper_ + {identificator} + _ + {credential}

```bash
# Openstack credentials: username, password, tenantName
# Identificator: defaults
mapper_defaults_username=fogbow
mapper_defaults_password=fogbow
mapper_defaults_tenantName=fogbow

# Identificator: other
# mapper_other_username=
# mapper_other_password=
# mapper_other_tenantName=

# Opennebula credentials: username, password
# Identificator: defaults
mapper_defaults_username=fogbow
mapper_defaults_password=fogbowpass

# EC2 credentals: accessKey, secretKey
# Identificator: defaults
mapper_defaults_accessKey=AKIALSKQLKFD7AHQLKEUO
mapper_defaults_secretKey=Iuaooiad&891/2309asd0123+akplkdh

# EC2 credentals: apiKey, secretKey
# Identificator: defaults
mapper_defaults_apiKey=user_api_key
mapper_defaults_secretKey=user_secret_key
```

##### Federation User Based Mapper Plugin
This mapping is done per intermediate of the user name logged like identificator.

```bash
# Mapper class
federation_user_credentail_class=org.fogbowcloud.manager.core.plugins.localcredentails.FederationUserBasedMapperPlugin

# Example 
# Identificator: fulado
mapper_fulano_username=fogbow
mapper_fulano_password=fogbow
mapper_fulano_tenantName=fogbow

# defaults
mapper_defaults_username=fogbowDefault
mapper_defaults_password=fogbowDefault
mapper_defaults_tenantName=fogbowDefaults
```
##### Member Based Mapper Plugin
This mapping is done per intermediate of the federation member that order the resource.

```bash
# Mapper class
federation_user_credentail_class=org.fogbowcloud.manager.core.plugins.localcredentails.MemberBasedMapperPlugin

# Example 
# Identificator: manager.com.br
mapper_manager.com.br_username=fogbow
mapper_manager.com.br_password=fogbow
mapper_manager.com.br_tenantName=fogbow

# defaults
mapper_defaults_username=fogbowDefault
mapper_defaults_password=fogbowDefault
mapper_defaults_tenantName=fogbowDefaults
```
##### Simple Mapper Plugin
This mapping is done only with the default.

```bash
# Mapper class
federation_user_credentail_class=org.fogbowcloud.manager.core.plugins.localcredentails.SingleMapperPlugin

# Example 
# Identificator: defaults
mapper_defaults_username=fogbow
mapper_defaults_defaults_password=fogbow
mapper_defaults_tenantName=fogbow
```
##### VOBased Mapper Plugin
This mapping is done with de CN of the VOMS user token.

```bash
# Mapper class
federation_user_credentail_class=org.fogbowcloud.manager.core.plugins.localcredentails.VOBasedMapperPlugin

# Example 
# Identificador: CN->Fulano,OU->DSC,O->UFCG,O->UFFBrGridCA,O->ICPEDU,C->BR
mapper_CN->Fulano,OU->DSC,O->UFCG,O->UFFBrGridCA,O->ICPEDU,C->BR_username=fogbow
mapper_CN->Fulano,OU->DSC,O->UFCG,O->UFFBrGridCA,O->ICPEDU,C->BR_password=fogbow
mapper_CN->Fulano,OU->DSC,O->UFCG,O->UFFBrGridCA,O->ICPEDU,C->BR_tenantName=fogbow

# defaults
mapper_defaults_username=fogbowDefault
mapper_defaults_defaults_password=fogbowDefault
mapper_defaults_tenantName=fogbowDefaults
```

## Prioritization Plugin
The Prioritization Plugin is responsible for prioritizing a request over another with lower priority.
It only comes into play when the quota for creating new resources is exceeded.
In these cases, it must verify if in that given time there is any fulfilled/ongoing request from a member with lower priority than the new requester.
If this condition is true, that request must preempted so that the new request can be met.

### Configure
##### Priotize Remove Order Plugin
Priotize the remote orders.

```bash
# Priorization Plugin class
remote_prioritization_plugin_class=org.fogbowcloud.manager.core.plugins.prioritization.PriotizeRemoteOrderPlugin
```
##### Two Fold Prioritization Plugin
This plugin allows a more refined prioritization policy by allowing that two other plugins be used by composition.
One is used for prioritization of locar orders, and the other for prioritization of remote orders.
```bash
# Priorization Plugin class
remote_prioritization_plugin_class=org.fogbowcloud.manager.core.plugins.prioritization.TwoFoldPrioritizationPlugin
```

##### Nof Prioritization Plugin
This plugin uses the Network of Favors as policy. Roughly, the Network of Favors prioritizes members from which it owes more favors.
```bash
# Priorization Plugin class
remote_prioritization_plugin_class=org.fogbowcloud.manager.core.plugins.prioritization.nof.NoFPrioritizationPlugin
nof_prioritize_local=true
nof_trustworthy=false
```

##### FCFS Prioritization Plugin
This plugin doesn't perform any prioritization. The first request to come is the first to be served. 
Obviously, the request is only met if there is free quota.
```bash
# Priorization Plugin class
local_prioritization_plugin_class=org.fogbowcloud.manager.core.plugins.prioritization.fcfs.FCFSPrioritizationPlugin

```
