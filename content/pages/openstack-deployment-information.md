Title: Openstack deployment information
url: openstack-deployment-information
save_as: openstack-deployment-information.html

Openstack deployment information
=====

For the infrastructure.conf.
------
{image}
* No dashboard Openstack * Compute (Right Menu) >> Access & Secure (Right Menu)  >> Compute (Main Content).
```
Example:
compute_novav2_url=http://truegrid-cloud:8774
```
* No dashboard Openstack * Compute (Right Menu) >> Access & Secure (Right Menu)  >> Image (Main Content)
```
Example:
compute_glancev2_url=http://truegrid-cloud:9292
```
* No dashboard Openstack * Compute (Right Menu) >> Access & Secure (Right Menu)  >> Storage (Main Content).
```
Example:
storage_v2_url=http://truegrid-cloud:8776
```
* No dashboard Openstack * Compute (Right Menu) >> Access & Secure (Right Menu)  >> Network (Main Content).
```
Example:
network_openstack_v2_url=http://truegrid-cloud:9696
```
* No dashboard Openstack * Compute (Right Menu) >> Access & Secure (Right Menu)  >> Identity (Main Content).
```
Example:
local_identity_url=http://truegrid-cloud:5000
If local_identity_class or  federation_identity_class is using KeystoneV3IdentityPlugin.  For more information in the Mapper section: http://www.fogbowcloud.org/interoperability-behavioral-plugins
```
* No dashboard Openstack * Identity (Right Menu) >> Users (Right Menu)  >> Choose the user (Main Content)
```
mapper_defaults_userId=$user_id
User password
mapper_defaults_password=$user_pass
No dashboard Openstack * Identity (Right Menu) >> Projects (Right Menu)  >> Choose your project (Main Content)
mapper_defaults_projectId=$project_id

If local_identity_class or  federation_identity_class is using KeystoneIdentityPlugin. For more information in the Mapper section: http://www.fogbowcloud.org/interoperability-behavioral-plugins
Username
mapper_defaults_username=$username
User password
mapper_defaults_password=$user_pass
Tenant Name
mapper_defaults_tenantName=$tenant_name
```
* No dashboard Openstack * Network (Right Menu) >> Networks (Right Menu) >> Choose the network default (Main Content).
```
Example:
compute_novav2_network_id=15054e12-0c41-4ed4-b2e1-2902a1ff0022
```
* No dashboard Openstack * Network (Right Menu) >> Networks (Right Menu) >> Choose the network external  (Main Content).
```
Example: 
external_gateway_info=d97a0b8e-574d-482a-8043-0cbf95a29f0a
```
* No dashboard Openstack * Compute (Right Menu) >> Images (Right Menu) >> Choose your image.
```
Example: 
image_storage_static_fogbow-ubuntu=95187035-52e3-4fe7-b178-1628615375bd
```

For the federation.conf.
------
* No dashboard Openstack * Compute (Right Menu) >> Access & Secure (Right Menu)  >> Identity (Main Content).
```
Example:
federation_identity_url=http://truegrid-cloud:5000
```
