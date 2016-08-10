Title: RESTful API
url: fogbow-api
save_as: fogbow-api.html
section: usage
index: 1

Programmatic interface
==========

Fogbow provides a RESTful API that implements OGF's OCCI standard. This API also extends the OCCI standard to incorporate fucntionalities that are only meaningful in the context of a federation of cloud providers.

### Resources

#### Order: /fogbow_request

Endpoint | Method | Header fields | Description
------------ | ------------- | ------------ | -------------
/fogbow_request | GET | **X-Auth-Token:** User's authentication token | Fetch the list of user's orders
/fogbow_request/{order_id} | GET | **X-Auth-Token:** User's authentication token | Fetch an order by its ID
/fogbow_request | DELETE | **X-Auth-Token:** User's authentication token | Delete all user's orders
/fogbow_request/{order_id} | DELETE | **X-Auth-Token:** User's authentication token | Delete a specific order by ID
/fogbow_request | POST | **X-Auth-Token:** User's authentication token<br>**X-OCCI-Attributes:** see list below <br> **Categories:** see list below | Description

##### Categories
The categories should be passed in the request headers with the key **Category** and the category name and value as its value, as in the example below.

`Category: categoryName=value`

Each category should be passed in a different header.

##### OCCI Attributes
The OCCI attributes should be passed in the request headers with the key **X-OCCI-Attribute** and the attribute name and value as its value, as in the example below.

`X-OCCI-Attribute: org.fogbowcloud.order.resource-kind=value`

Each attribute should be passed in a different header.

OCCI Attributes for Order

Attribute name | Type | Description
------------ | ------------ | ------------
org.fogbowcloud.request.instance-count | int | Number of instances to be created
org.fogbowcloud.request.type | string | Type of the request: one-time or persistent
org.fogbowcloud.request.extra-user-data | string | Optional user data in cloud-init format
org.fogbowcloud.request.extra-user-data-content-type | string | Type of the user data as specified in cloud-init documentation
org.fogbowcloud.order.storage-size | int | Storage size in GB
occi.network.address | string | Network address in CIDR notation
occi.network.gateway | string | Network gateway
occi.network.allocation | string | Accepted values: dynamic or static
org.fogbowcloud.order.resource-kind | string | Kind of resource to be created: compute, storage or network
org.fogbowcloud.request.requirements | string | Expression with minimum requirements to create resources

#### Compute: /compute

Endpoint | Method | Header fields | Description
------------ | ------------- | ------------ | -------------
/compute | GET | **X-Auth-Token:** User's authentication token | Fetch the list of user's computes
/compute/{compute_id} | GET | **X-Auth-Token:** User's authentication token | Fetch an compute by its ID
/compute | DELETE | **X-Auth-Token:** User's authentication token | Delete all user's computes
/compute/{compute_id} | DELETE | **X-Auth-Token:** User's authentication token | Delete a specific compute by ID
/compute | POST | **X-Auth-Token:** User's authentication token<br>**X-OCCI-Attributes:** see list below <br> **Categories:** see list below | Description

#### Storage: /storage

Endpoint | Method | Header fields | Description
------------ | ------------- | ------------ | -------------
/storage | GET | **X-Auth-Token:** User's authentication token | Fetch the list of user's storages
/storage/{storage_id} | GET | **X-Auth-Token:** User's authentication token | Fetch an storage by its ID
/storage/{storage_id} | DELETE | **X-Auth-Token:** User's authentication token | Delete a specific storage by ID
/storage | POST | **X-Auth-Token:** User's authentication token<br>**X-OCCI-Attributes:** see list below <br> **Categories:** see list below | Create a storage


OCCI Categories for Storage

Category name  | Description
------------ | ------------
storage; scheme="http://schemas.ogf.org/occi/infrastructure#"; class="kind" | Storage category


OCCI Attributes for Storage

Attribute name | Type | Description
------------ | ------------ | ------------
occi.storage.size | int | Storage size

#### Network: /network

Endpoint | Method | Header fields | Description
------------ | ------------- | ------------ | -------------
/network | GET | **X-Auth-Token:** User's authentication token | Fetch the list of user's networks
/network/{network_id} | GET | **X-Auth-Token:** User's authentication token | Fetch an network by its ID
/network/{network_id} | DELETE | **X-Auth-Token:** User's authentication token | Delete a specific network by ID
/network | POST | **X-Auth-Token:** User's authentication token<br>**X-OCCI-Attributes:** see list below <br> **Categories:** see list below | Create a network


OCCI Categories for Network

Category name  | Description
------------ | ------------
network; scheme="http://schemas.ogf.org/occi/infrastructure#"; class="kind" | Storage category


OCCI Attributes for Network

Attribute name | Type | Description
------------ | ------------ | ------------
occi.network.address | String | CIRD - ###.###.###.###/##
occi.network.gateway | String | ###.###.###.###

#### Attachment: /storage/link

Endpoint | Method | Header fields | Description
------------ | ------------- | ------------ | -------------
/storage/link | GET | **X-Auth-Token:** User's authentication token | Fetch the list of user's attachments
/storage/link/{storagelink_id} | GET | **X-Auth-Token:** User's authentication token | Fetch an attachment by its ID
/storage/link/{storagelink_id} | DELETE | **X-Auth-Token:** User's authentication token | Delete a specific attachment by ID
/storage/link/ | POST | **X-Auth-Token:** User's authentication token<br>**X-OCCI-Attributes:** see list below <br> **Categories:** see list below | Create a attachment

OCCI Categories for attachmente

Category name  | Description
------------ | ------------
storagelink; scheme="http://schemas.ogf.org/occi/infrastructure#"; class="kind" | Storage category


OCCI Attributes for attachment

Attribute name | Type | Description
------------ | ------------ | ------------
occi.core.source | String | Compute id
occi.core.target | String | Storage id

#### Members: /member

Endpoint | Method | Header fields | Description
------------ | ------------- | ------------ | -------------
/member | GET | **X-Auth-Token:** User's authentication token | Fetch the list of federation members
/member/{member_id}/quota | GET | **X-Auth-Token:** User's authentication token | Fetch an quota of member by its ID
/member/{member_id}/usage | GET | **X-Auth-Token:** User's authentication token | Fetch an usage of member by its ID

#### Members: /member

Endpoint | Method | Header fields | Description
------------ | ------------- | ------------ | -------------
/member | GET | **X-Auth-Token:** User's authentication token | Fetch the list of federation members
/member/{member_id}/quota | GET | **X-Auth-Token:** User's authentication token | Fetch an quota of member by its ID
/member/{member_id}/usage | GET | **X-Auth-Token:** User's authentication token | Fetch an usage of member by its ID
/member/accounting/compute | GET | **X-Auth-Token:** User's authentication token | Fetch a accounting of compute
/member/accounting/compute | GET | **X-Auth-Token:** User's authentication token | Fetch a accounting of storage
