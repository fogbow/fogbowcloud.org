Title: RESTful API
url: fogbow-api
save_as: fogbow-api.html
section: usage
index: 1

Programmatic interface
==========

Fogbow provides a RESTful API that implements OGF's OCCI standard. This API also extends the OCCI standard to incorporate fucntionalities that are only meaningful in the context of a federation of cloud providers.

#### Categories
The categories should be passed in the request headers with the key **Category** and the value, as in the example below.

`Category: {category value}`

Each category should be passed in a different header.

#### Links
The links should be passed in the request headers with the key **Link** and the value, as in the example below.

`Link: {link value}`

Each link should be passed in a different header.

#### OCCI Attributes
The OCCI attributes should be passed in the request headers with the key **X-OCCI-Attribute** and the attribute name and value as its value, as in the example below.

`X-OCCI-Attribute: org.fogbowcloud.order.resource-kind=value`

Each attribute should be passed in a different header.

## Resources

### Order: /order

Endpoint | Method | Header fields | Description
------------ | ------------- | ------------ | -------------
/order | GET | **X-Auth-Token:** User's authentication token | Fetch the list of user's orders
/order/\{order_id\} | GET | **X-Auth-Token:** User's authentication token | Fetch an order by its ID
/order | DELETE | **X-Auth-Token:** User's authentication token | Delete all user's orders
/order/\{order_id\} | DELETE | **X-Auth-Token:** User's authentication token | Delete a specific order by ID
/order | POST | **X-Auth-Token:** User's authentication token<br>**X-OCCI-Attributes**: Attributes<br>**Link**: Links<br>**Category**: Categories
 | | |

#### OCCI Categories for Order

Category name  | Required | Description
------------ | ------------ | ------------
order; scheme="http://schemas.fogbowcloud.org/request#"; class="kind" | required | Compute category
flavor_name; scheme="http://schemas.fogbowcloud.org/template/resource#"; class="mixin" | optional | Flavor name category
\{image_name\}; scheme="http://schemas.fogbowcloud.org/template/os#"; class="mixin" | required for compute | Image name category
fogbow_public_key; scheme="http://schemas.fogbowcloud/credentials#"; class="mixin" | optional | Public key category
 | | 

#### OCCI Link for Order

Link name  | Required | Description
------------ | ------------ | ------------
</ network/network_value >; rel="http://schemas.ogf.org/occi/infrastructure#network"; category="http://schemas.ogf.org/occi/infrastructure#network" | optional | Link 
 | | 

#### OCCI Attributes for Order

Attribute name | Type | Required | Description
------------ | ------------ | ------------ | ------------
org.fogbowcloud.request.instance-count | int | required | Number of instances to be created
org.fogbowcloud.request.type | string | optional | Type of the request: one-time or persistent
org.fogbowcloud.request.extra-user-data | string | optional | <a  href="http://cloudinit.readthedocs.io/en/latest/topics/format.html" target="_blank">User data in cloud-init format</a>. Base64 encoded
org.fogbowcloud.request.extra-user-data-content-type | string | optional | Type of the user data as specified in cloud-init documentation
org.fogbowcloud.order.storage-size | int | required for storage | Storage size in GB
occi.network.address | string | required for network | Network address in CIDR notation
occi.network.gateway | string | optional | Network gateway
occi.network.allocation | string | optional | Accepted values: dynamic or static
org.fogbowcloud.order.resource-kind | string | required | Kind of resource to be created: compute, storage or network
org.fogbowcloud.request.requirements | string | optional | Expression with minimum requirements to create resources<br>(See below)
org.openstack.credentials.publickey.data | string | optional | Public key data
org.openstack.credentials.publickey.name | string | optional | Public key name
 | | |

#### Requirements
Requirements is an expression containing information used on the creation of an instance.
The requirements are separated by `&&`.

Requirement | Used in order of type | Description
----------- | --------------------- | -----------
Glue2CloudComputeManagerID | **compute**<br>**storage**<br>**network** | Sets on which manager the order is to be used
Glue2vCPU | **compute** | Sets the number of vCPUs
Glue2RAM | **compute** | Sets the amount of vRAM
 | |

Example
```
"Glue2CloudComputeManagerID=="manager-id" && Glue2vCPU >= 1 && Glue2RAM >= 1024"
```

<br>
#### Examples:

Create order type compute.
``` shell
POST /order
Category: order; scheme="http://schemas.fogbowcloud.org/request#"; class="kind"   
Category: fogbow_small; scheme="http://schemas.fogbowcloud.org/template/resource#"; class="mixin"
Category: fogbow-ubuntu; scheme="http://schemas.fogbowcloud.org/template/os#"; class="mixin"   
Category: fogbow_public_key; scheme="http://schemas.fogbowcloud.org/credentials#"; class="mixin"   
X-OCCI-Attribute: org.fogbowcloud.request.instance-count=1
X-OCCI-Attribute: org.fogbowcloud.request.type=one-time
X-OCCI-Attribute: org.fogbowcloud.request.extra-user-data={base64 encoded script}
X-OCCI-Attribute: org.fogbowcloud.request.extra-user-data-content-type=text/x-shellscript
X-OCCI-Attribute: org.fogbowcloud.order.resource-kind=compute
X-OCCI-Attribute: org.fogbowcloud.request.requirements="Glue2CloudComputeManagerID==\"manager.one.member.com\"" 
X-OCCI-Attribute: org.openstack.credentials.publickey.data={public_key}
X-OCCI-Attribute: org.openstack.credentials.publickey.name=mypublickey
</ network/network00 >; rel="http://schemas.ogf.org/occi/infrastructure#network"; category="http://schemas.ogf.org/occi/infrastructure#network"
```

Create order type storage.
``` shell
POST /order
Category: order; scheme="http://schemas.fogbowcloud.org/request#"; class="kind"
X-OCCI-Attribute: org.fogbowcloud.request.instance-count=1
X-OCCI-Attribute: org.fogbowcloud.request.type=one-time
X-OCCI-Attribute: org.fogbowcloud.order.resource-kind=storage
X-OCCI-Attribute: org.fogbowcloud.order.storage-size=10
```

Create order type network.
``` shell
POST /order
Category: order; scheme="http://schemas.fogbowcloud.org/request#"; class="kind"
X-OCCI-Attribute: org.fogbowcloud.request.instance-count=1
X-OCCI-Attribute: org.fogbowcloud.request.type=one-time
X-OCCI-Attribute: org.fogbowcloud.order.resource-kind=network
X-OCCI-Attribute: occi.network.address=10.10.10.10/24
X-OCCI-Attribute: occi.network.gateway=10.10.10.1
X-OCCI-Attribute: occi.network.allocation=dynamic
```

### Compute: /compute

Endpoint | Method | Header fields | Description
------------ | ------------- | ------------ | -------------
/compute | GET | **X-Auth-Token:** User's authentication token | Fetch the list of user's computes
/compute/\{compute_id\} | GET | **X-Auth-Token:** User's authentication token | Fetch an compute by its ID
/compute/\{compute_id\} | DELETE | **X-Auth-Token:** User's authentication token | Delete a specific compute by ID
/compute<br>**(DEPRECATED)** | POST | **X-Auth-Token:** User's authentication token<br>**X-OCCI-Attributes**: Attributes<br>**Categories**: Categories | Create a compute
 | | |

#### OCCI Categories for Compute

Category name  | required | Description
------------ | ------------ | ------------
compute; scheme="http://schemas.ogf.org/occi/infrastructure#"; class="kind" | required | Compute category
flavor_name; scheme="http://schemas.fogbowcloud.org/template/resource#"; class="mixin" | required | Flavor name category
image_name; scheme="http://schemas.fogbowcloud.org/template/os#"; class="mixin" | required | Image name category
 | |

#### OCCI Attributes for Compute

Attribute name | Type | required | Description
------------ | ------------ | ------------ | ------------
org.fogbowcloud.request.extra-user-data | string | optional | <a  href="http://cloudinit.readthedocs.io/en/latest/topics/format.html" target="_blank">User data in cloud-init format</a>. Base64 encoded
org.fogbowcloud.request.extra-user-data-content-type | string | optional | Type of the user data as specified in cloud-init documentation
org.openstack.credentials.publickey.data | string | optional | Public key data
org.openstack.credentials.publickey.name | string | optional | Public key name
 | | |

### Storage: /storage

Endpoint | Method | Header fields | Description
------------ | ------------- | ------------ | -------------
/storage | GET | **X-Auth-Token:** User's authentication token | Fetch the list of user's storages
/storage/\{storage_id\} | GET | **X-Auth-Token:** User's authentication token | Fetch an storage by its ID
/storage/\{storage_id\} | DELETE | **X-Auth-Token:** User's authentication token | Delete a specific storage by ID
/storage | POST | **X-Auth-Token:** User's authentication token<br>**X-OCCI-Attributes**: Attributes<br>**Categories**: Categories | Create a storage
 | | |


#### OCCI Categories for Storage

Category name  | required | Description
------------ | ------------ | ------------
storage; scheme="http://schemas.ogf.org/occi/infrastructure#"; class="kind" | required | Storage category
 | | 


#### OCCI Attributes for Storage

Attribute name | Type | required | Description
------------ | ------------ | ------------ | ------------
occi.storage.size | int | required | Storage size
 | | |

### Network: /network

Endpoint | Method | Header fields | Description
------------ | ------------- | ------------ | -------------
/network | GET | **X-Auth-Token:** User's authentication token | Fetch the list of user's networks
/network/\{network_id\} | GET | **X-Auth-Token:** User's authentication token | Fetch an network by its ID
/network/\{network_id\} | DELETE | **X-Auth-Token:** User's authentication token | Delete a specific network by ID
/network | POST | **X-Auth-Token:** User's authentication token<br>**X-OCCI-Attributes**: Attributes<br>**Categories**: Categories | Create a network
 | | |


#### OCCI Categories for Network

Category name  | required | Description
------------ | ------------ | ------------
network; scheme="http://schemas.ogf.org/occi/infrastructure#"; class="kind" | required | Storage category
 | | 

#### OCCI Attributes for Network

Attribute name | Type | required | Description
------------ | ------------ | ------------ | ------------
occi.network.address | String | required | IP address in CIDR notation
occi.network.gateway | String | optional | IP address of network gateway
 | | |

### Attachment: /storage/link

Endpoint | Method | Header fields | Description
------------ | ------------- | ------------ | -------------
/storage/link | GET | **X-Auth-Token:** User's authentication token | Fetch the list of user's attachments
/storage/link/\{storagelink_id\} | GET | **X-Auth-Token:** User's authentication token | Fetch an attachment by its ID
/storage/link/\{storagelink_id\} | DELETE | **X-Auth-Token:** User's authentication token | Delete a specific attachment by ID
/storage/link/ | POST | **X-Auth-Token:** User's authentication token<br>**X-OCCI-Attributes**: Attributes<br>**Categories**: Categories | Create a attachment
 | | |

#### OCCI Categories for attachment

Category name  | required | Description
------------ | ------------ | ------------
storagelink; scheme="http://schemas.ogf.org/occi/infrastructure#"; class="kind" | required | Storage category
 | | 

#### OCCI Attributes for attachment

Attribute name | Type | required | Description
------------ | ------------ | ------------ | ------------
occi.core.source | String | required | Compute id
occi.core.target | String | required | Storage id
 | | |

### Members: /member

<!-- <svg width="34" height="17">
    <rect x="0" y="0" rx="2" ry="2" width="34" height="16" style="fill:green;opacity:0.5" />
    <text x="3" y="13" fill="white" font-family="Courier New">GET</text>
</svg> /member -->

Endpoint | Method | Header fields | Description
------------ | ------------- | ------------ | -------------
/member | GET | **X-Auth-Token:** User's authentication token | Fetch the list of federation members
/member/\{member_id\}/quota | GET | **X-Auth-Token:** User's authentication token | Fetch an quota of member by it's ID
/member/\{member_id\}/usage | GET | **X-Auth-Token:** User's authentication token | Fetch an usage of member by it's ID
/member/accounting/compute | GET | **X-Auth-Token:** User's authentication token | Fetch a accounting of compute
/member/accounting/storage | GET | **X-Auth-Token:** User's authentication token | Fetch a accounting of storage
 | | |
