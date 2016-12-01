Title: Openstack deployment information
url: openstack-deployment-information
save_as: openstack-deployment-information.html
index: 0

Openstack deployment information
=====

Below we show where to find in the OpenStack dashboard the information to configure the Fogbow Manager (through the **infrastructure.conf** and the **federation.conf** files).

infrastructure.conf properties
------

The image below (starting from the right menu, then following the links: compute, access & secure and compute) shows the URls of OpenStack APIs. Based on this information, we can configure these properties:

```
(based on compute field)
compute_novav2_url=http://truegrid-cloud:8774

(based on image field)
compute_glancev2_url=http://truegrid-cloud:9292

(based on storage field)
storage_v2_url=http://truegrid-cloud:8776

(based on the network field)
network_openstack_v2_url=http://truegrid-cloud:9696
```

![alt logo](../images/openstack-accessand-security.png "Openstack Access & Security")

* No dashboard Openstack * Compute (Right Menu) >> Access & Secure (Right Menu)  >> Identity (Main Content).

```
Example:
local_identity_url=http://truegrid-cloud:5000
If local_identity_class or  federation_identity_class is using KeystoneV3IdentityPlugin.  For more information in the Mapper section: http://www.fogbowcloud.org/interoperability-behavioral-plugins
```
* No dashboard Openstack * Identity (Right Menu) >> Users (Right Menu)  >> Choose the user (Main Content)

![alt logo](../images/openstack-project.png "Openstack Projects")
```
No dashboard Openstack * Identity (Right Menu) >> Users (Right Menu)  >> Choose your user (Main Content)
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

![alt logo](../images/openstack-network-details.png "Openstack Network")
```
Example:
compute_novav2_network_id=15054e12-0c41-4ed4-b2e1-2902a1ff0022
```
* No dashboard Openstack * Network (Right Menu) >> Networks (Right Menu) >> Choose the network external  (Main Content).

![alt logo](../images/openstack-network-external-detail.png "Openstack Network External")
```
Example: 
external_gateway_info=d97a0b8e-574d-482a-8043-0cbf95a29f0a
```
* No dashboard Openstack * Compute (Right Menu) >> Images (Right Menu) >> Choose your image.

![alt logo](../images/openstack-image-details.png "Openstack Image Details")
```
Example: 
image_storage_static_fogbow-ubuntu=95187035-52e3-4fe7-b178-1628615375bd
```

For the federation.conf.
------
* No dashboard Openstack * Compute (Right Menu) >> Access & Secure (Right Menu)  >> Identity (Main Content).

![alt logo](../images/openstack-accessand-security.png "Openstack Access & Security")
```
Example:
federation_identity_url=http://truegrid-cloud:5000
```
