Title: Fogbow CLI
url: fogbow-cli
save_as: fogbow-cli.html
section: usage
index: 2

Fogbow CLI
==========

The fogbow CLI is a command line interface for the fogbow manager. It makes it easier for fogbow users to create HTTP requests and invoke them through the manager's OCCI API. Through the fogbow CLI, users are able to get information about federation members; create, retrive and delete instance, network, storage and attachment; and create orders.

##Installation
Follow these steps, <a  href="/install-configure-fogbow-cli" target="_blank">Instalation and configuration Fogbow cli</a> .

## Member operations (```member```)

### List federation members 
Get all federation members.

* **--url** (optional; default: http://localhost:8182): OCCI endpoint
* **--auth-token** (user's token/Text)  or **--auth-file** (user's token/Path); (required)

Example:
```bash
$ fogbow-cli member --url http://localhost:8182 --auth-file /tmp/token

federation.member.one.com
federation.member.two.com
federation.member.three.com
```

### Get quota
Get the quota of the federation member

* **--url** (optional; default: http://localhost:8182): OCCI endpoint
* **--quota** (required)
* **--memberId** (required)
* **--auth-token** (user's token/Text)  or **--auth-file** (user's token/Path); (required)

Example:
```bash
$ fogbow-cli member --url http://localhost:8182 --quota --memberId id123 --auth-token mytoken

cpuQuota=1;cpuInUse=1;cpuInUseByUser=1;memQuota=1;memInUse=1;memInUseByUser=1;instancesQuota=1;instancesInUse=1;instancesInUseByUser=1
```

### Get usage
Get the usage of the federation member

* **--url** (optional; default: http://localhost:8182): OCCI endpoint
* **--usage** (required)
* **--memberId** (required)
* **--auth-token** (user's token/Text)  or **--auth-file** (user's token/Path); (required)

Example:
```bash
$ fogbow-cli member --url http://localhost:8182 --usage --memberId id123 --auth-token mytoken

memberId=federation.member.one.com
compute usage=10
storage usage=10
```

## Resource operations (```resource```)

### Get all fogbow resources 

Get all OCCI resources provided by fogbow. 

* **--get** (required)
* **--url** (optional; default: http://localhost:8182): OCCI endpoint
* **--auth-token** (user's token/Text)  or **--auth-file** (user's token/Path); (required)

Example:
```bash
$ fogbow-cli resource --get --url http://localhost:8182 --auth-token mytoken

Category: fogbow-request; scheme="http://schemas.fogbowcloud.org/request#"; class="kind"; title="Request new Instances"; location="http://localhost:8182/request"; attributes="org.fogbowcloud.request.instance-count org.fogbowcloud.request.type org.fogbowcloud.request.valid-until org.fogbowcloud.request.valid-from"
Category: fogbow-large; scheme="http://scmhemas.fogbowcloud.org/template/resource#"; class="mixin"; title="Large Flavor"; location="http://localhost:8182/large"
Category: fogbow-linux-x86; scheme="http://schemas.fogbowcloud.org/template/os#"; class="mixin"; title="Linux-x86 Image"; location="http://localhost:8182/fogbow-linux-x86"
...
```

## Token operations (```token```)

### Create a new Token
Create a new user token.

Note: to pass the credentials and the identity plugin endpoint it is necessary the use of dynamic parameters; follow the example with the OpenStack credentials:

* **--create** (required)
* **--type** (required) : identity plugin type
* **-DauthUrl** (required): identity plugin endpoint
* **-Dpassword=** (optional): dynamic parameter
* **-Dusername=** (optional): dynamic parameter
* **-DtenantName=** (optional): dynamic parameter

Example:
```bash
$ fogbow-cli token --create -Dpassword=mypassword -Dusername=myusername -DtenantName=mytenantname -DauthUrl=http://localhost:8182 --type openstack

MIINXgYJKoZIhvcNAQcCoIINTzCCDUsCAQExCTAHBgUrDgMCGjCCC7QGCSqGSIb3DQEHAaCCC6UEgguheyJhY2Nlc3MiOiB7InRva2VuIjogeyJpc3N1ZWRfYXQiOiAiMjAxNC0wNS0
```

Others credentails:
```bash
* x509
   -Dx509CertificatePath (Required)
* keystone
   -Dusername (Required)
   -Dpassword (Required)
   -DtenantName (Required)
   -DauthUrl (Required)
* voms
   -Dpassword (Required)
   -DserverName (Required)
   -DpathUserCred (Optional) - default :$HOME/.globus
   -DpathUserKey (Optional) - default :$HOME/.globus
* opennebula
   -Dusername (Required)
   -Dpassword (Required)
```

## Request operations (```request```)

### Get order 
Get all instance orders associated to a particular user's token.

* **--get** (required)
* **--url** (optional; default: http://localhost:8182): OCCI endpoint
* **--auth-token** (user's token/Text)  or **--auth-file** (user's token/Path); (required)

Example:
```bash
$ fogbow-cli order --get --auth-token mytoken --url http://localhost:8182

X-OCCI-Location: http://localhost:8182/request/47536d31-0674-4278-ad05-eff5fdd07257
X-OCCI-Location: http://localhost:8182/request/fd745806-4909-4a39-8380-13183b1f197c
```

Get detailed information about a single instance order.
* **--get** (required)
* **--url** (optional; default: http://localhost:8182): OCCI endpoint
* **--auth-token** (user's token/Text)  or **--auth-file** (user's token/Path); (required)
* **--id** (required): order id

Example:
```bash
$ fogbow-cli order --get --auth-token mytoken --id orderid --url http://localhost:8182

Category: fogbow_request; scheme="http://schemas.fogbowcloud.org/request#"; class="kind"; title="Order new Instances"; rel="http://schemas.ogf.org/occi/core#resource"; location="http://localhost:8182/fogbow_request/"; attributes="org.fogbowcloud.request.instance-count org.fogbowcloud.request.type org.fogbowcloud.request.valid-until org.fogbowcloud.request.valid-from org.fogbowcloud.request.state org.fogbowcloud.request.instance-id org.fogbowcloud.credentials.publickey.data org.fogbowcloud.request.user-data org.fogbowcloud.request.extra-user-data org.fogbowcloud.request.extra-user-data-content-type org.fogbowcloud.request.requirements org.fogbowcloud.request.batch-id org.fogbowcloud.request.requesting-member org.fogbowcloud.request.providing-member org.fogbowcloud.order.resource-kind org.fogbowcloud.order.storage-size"
X-OCCI-Attribute: org.fogbowcloud.request.extra-user-data="Not defined" 
X-OCCI-Attribute: org.fogbowcloud.request.state="fulfilled" 
X-OCCI-Attribute: org.fogbowcloud.request.valid-from="Not defined" 
X-OCCI-Attribute: org.fogbowcloud.request.requirements="Glue2CloudComputeManagerID=="manager.one.member.com"" 
X-OCCI-Attribute: occi.core.id="c575e6f2-a590-4595-8894-02a8e63bd214" 
X-OCCI-Attribute: org.fogbowcloud.request.type="one-time" 
X-OCCI-Attribute: org.fogbowcloud.request.valid-until="Not defined" 
X-OCCI-Attribute: org.fogbowcloud.request.providing-member="manager.one.member.com" 
X-OCCI-Attribute: org.fogbowcloud.credentials.publickey.data="Not defined" 
X-OCCI-Attribute: org.fogbowcloud.order.resource-kind="storage" 
X-OCCI-Attribute: org.fogbowcloud.request.requesting-member="manager.two.member.com" 
X-OCCI-Attribute: org.fogbowcloud.request.extra-user-data-content-type="Not defined" 
X-OCCI-Attribute: org.fogbowcloud.request.user-data="Not defined" 
X-OCCI-Attribute: org.fogbowcloud.order.storage-size="80" 
X-OCCI-Attribute: org.fogbowcloud.request.batch-id="25a0fd51-7b47-452a-9daa-ef40d1277f80" 
X-OCCI-Attribute: org.fogbowcloud.request.instance-count="1" 
X-OCCI-Attribute: org.fogbowcloud.request.instance-id="9e80c942-9fbd-4c06-b8b7-ed7573544425@manager.one.member.com"
```

### Create order 
Create instance orders.

* **--create** (required)
* **--url** (optional; default: http://localhost:8182): OCCI endpoint
* **--auth-token** (user's token/Text)  or **--auth-file** (user's token/Path); (required)
* **--n** (optional; default: 1): number of orders to be created
* **--image** (optional; default: fogbow-linux-x86): fogbow image
* **--flavor** (optional; default: fogbow-small): fogbow flavor
* **--public-key** (optional; path public key): fogbow public key
* **--requirements** (optinal): Requirements of the flavor and about location where the instance will be provide

Example:
```bash
$ fogbow-cli order --create --n 2 --image fogbow-linux-x86 --flavor large --url http://localhost:8182 --public-key ~/.ssh/id_rsa.pub --requirements "Glue2RAM >= 1024 && Glue2CloudComputeManagerID==\"manager.one.member.com\""

X-OCCI-Location: http://localhost:8182/request/47536d31-0674-4278-ad05-eff5fdd07257
X-OCCI-Location: http://localhost:8182/request/fd745806-4909-4a39-8380-13183b1f197c
```

Example using defaults:
```bash
$ fogbow-cli order --create --url http://localhost:8182

X-OCCI-Location: http://localhost:8182/request/47536d31-0674-4278-ad05-eff5fdd07257
```

### Delete a single order
Delete a single instance order.

* **--delete** (required)
* **--url** (optional; default: http://localhost:8182): OCCI endpoint
* **--auth-token** (user's token/Text)  or **--auth-file** (user's token/Path); (required)
* **--id** (required): order id

Example:
```bash
$ fogbow-cli order --delete --auth-token mytoken --id orderid --url http://localhost:8182

Ok
```

## Instance operations (```instance```)

### Get all instances
Get all instances associated to a particular user's token.

* **--get**  (required)
* **--url** (optional; default: http://localhost:8182): OCCI endpoint
* **--auth-token** (user's token/Text)  or **--auth-file** (user's token/Path); (required)

Example:
```bash
$ fogbow-cli instance --get --auth-token  mytoken --url http://localhost:8182

X-OCCI-Location: 3I235356-3432434-324324-3243242f@manager.one.com
X-OCCI-Location: 4B869582-8907667-123457-0765345c@manager.one.com
```

### Get a single instance
Get detailed information about a single instance.

* **--get**  (required)
* **--url** (optional; default: http://localhost:8182): OCCI endpoint
* **--auth-token** (user's token/Text)  or **--auth-file** (user's token/Path); (required)
* **--id** (required): instance id

Example: 
```bash
$ fogbow-cli instance --get --auth-token mytoken --id instanceid --url http://localhost:10000

Category: compute; scheme="http://schemas.ogf.org/occi/infrastructure#"; class="kind"; title="Compute Resource"; rel="http://schemas.ogf.org/occi/core#resource"; location="http://localhost:8182/compute/"; attributes="occi.compute.architecture occi.compute.state{immutable} occi.compute.speed occi.compute.memory occi.compute.cores occi.compute.hostname"; actions="http://schemas.ogf.org/occi/infrastructure/compute/action#start http://schemas.ogf.org/occi/infrastructure/compute/action#stop http://schemas.ogf.org/occi/infrastructure/compute/action#restart http://schemas.ogf.org/occi/infrastructure/compute/action#suspend"
Category: os_tpl; scheme="http://schemas.ogf.org/occi/infrastructure#"; class="mixin"; location="http://localhost:8182/os_tpl/"
Category: flavor.small; scheme="http://schemas.fogbowcloud.org/template/resource#"; class="mixin"; title="flavor.small"; rel="http://schemas.ogf.org/occi/infrastructure#resource_tpl"; location="http://localhost:8182/flavor.small/"
Category: fogbow-image; scheme="http://schemas.fogbowcloud.org/template/os#"; class="mixin"; title="fogbow-image image"; rel="http://schemas.ogf.org/occi/infrastructure#os_tpl"; location="http://localhost:8182/fogbow-image/"
X-OCCI-Attribute: occi.compute.state="active"
X-OCCI-Attribute: occi.compute.hostname="fogbow-instance-230e00cd-4207-4d77-b40c-ba7ca8fa52fd"
X-OCCI-Attribute: org.fogbowcloud.request.ssh-username="fogbow"
X-OCCI-Attribute: occi.compute.memory="2.0"
X-OCCI-Attribute: org.fogbowcloud.request.local-ip-address="0.0.0.0"
X-OCCI-Attribute: org.fogbowcloud.request.extra-ports="{"postgres":"0.0.0.0:10029"}"
X-OCCI-Attribute: occi.compute.cores="1"
X-OCCI-Attribute: org.fogbowcloud.request.ssh-public-address="0.0.0.0:10028"
X-OCCI-Attribute: occi.core.id="4090901b-3b82-4424-a93f-5425e59dee71"
X-OCCI-Attribute: occi.compute.architecture="Not defined"
X-OCCI-Attribute: occi.compute.speed="Not defined"
```

### Delete all instances
Delete all instances associated to a particular user's token.

* **--delete**  (required)
* **--url** (optional; default: http://localhost:8182): OCCI endpoint
* **--auth-token** (user's token/Text)  or **--auth-file** (user's token/Path); (required)

```bash
$ fogbow-cli instance --delete --auth-token mytoken --url http://localhost:8182

Ok
```

### Delete a single instance
Delete a single instance.

* **--delete** (required)
* **--url** (optional; default: http://localhost:8182): OCCI endpoint
* **--auth-token** (user's token/Text)  or **--auth-file** (user's token/Path); (required)
* **--id** (required): instance id

```bash
$ fogbow-cli instance --delete --auth-token mytoken --id instanceid --url http://localhost:8182

Ok
```

### Create instance
Create a instance

* **--create** (required)
* **--url** (optional; default: http://localhost:8182): OCCI endpoint
* **--auth-token** (user's token/Text)  or **--auth-file** (user's token/Path); (required)
* **--flavor** (optional) 
* **--image** (required)
* **--publicKey** (optional)
* **--userDataFile** (optional)

```bash
$ fogbow-cli instance --create --auth-token mytoken --url http://localhost:8182 --flavor m1-small --image fogbow-ubuntu --publicKey /tmp/pkey --userDataFile /tmp/userdatafile

X-OCCI-Location: federated_instance_230e00cd-4207-4d77-b40c-ba7ca8fa52fd
```

## Storage operations (```storage```)

### Get all storages
Get all storages associated to a particular user's token.

* **--get**  (required)
* **--url** (optional; default: http://localhost:8182): OCCI endpoint
* **--auth-token** (user's token/Text)  or **--auth-file** (user's token/Path); (required)

Example:
```bash
$ fogbow-cli storage --get --auth-token  mytoken --url http://localhost:8182

X-OCCI-Location: 3I235356-3432434-324324-3243242f@manager.one.com
X-OCCI-Location: 4B869582-8907667-123457-0765345c@manager.one.com
```

### Get a single storage
Get detailed information about a single storage.

* **--get**  (required)
* **--url** (optional; default: http://localhost:8182): OCCI endpoint
* **--auth-token** (user's token/Text)  or **--auth-file** (user's token/Path); (required)
* **--id** (required): storage id

Example: 
```bash
$ fogbow-cli storage --get --auth-token mytoken --id storageid --url http://localhost:10000


```

### Delete all storages
Delete all storages associated to a particular user's token.

* **--delete**  (required)
* **--url** (optional; default: http://localhost:8182): OCCI endpoint
* **--auth-token** (user's token/Text)  or **--auth-file** (user's token/Path); (required)

```bash
$ fogbow-cli storage --delete --auth-token mytoken --url http://localhost:8182

Ok
```

### Delete a single storage
Delete a single storage.

* **--delete** (required)
* **--url** (optional; default: http://localhost:8182): OCCI endpoint
* **--auth-token** (user's token/Text)  or **--auth-file** (user's token/Path); (required)
* **--id** (required): storage id

```bash
$ fogbow-cli instance --delete --auth-token mytoken --id storageid --url http://localhost:8182

Ok
```

### Create storage
Create a storage

* **--create** (required)
* **--url** (optional; default: http://localhost:8182): OCCI endpoint
* **--auth-token** (user's token/Text)  or **--auth-file** (user's token/Path); (required)
* **--size** (required; Unit GB) 

```bash
$ fogbow-cli storage --create --auth-token mytoken --url http://localhost:8182 --size 20

X-OCCI-Location: federated_instance_230e00cd-4207-4d77-b40c-ba7ca8fa52fd
```

## Network operations (```network```)

### Get all networks
Get all networks associated to a particular user's token.

* **--get**  (required)
* **--url** (optional; default: http://localhost:8182): OCCI endpoint
* **--auth-token** (user's token/Text)  or **--auth-file** (user's token/Path); (required)

Example:
```bash
$ fogbow-cli network --get --auth-token  mytoken --url http://localhost:8182

X-OCCI-Location: 3I235356-3432434-324324-3243242f
X-OCCI-Location: 4B869582-8907667-123457-0765345c
```

### Get a single network
Get detailed information about a single network.

* **--get**  (required)
* **--url** (optional; default: http://localhost:8182): OCCI endpoint
* **--auth-token** (user's token/Text)  or **--auth-file** (user's token/Path); (required)
* **--id** (required): network id

Example: 
```bash
$ fogbow-cli network --get --auth-token mytoken --id networkid --url http://localhost:10000

...
```

### Delete all networks
Delete all networks associated to a particular user's token.

* **--delete**  (required)
* **--url** (optional; default: http://localhost:8182): OCCI endpoint
* **--auth-token** (user's token/Text)  or **--auth-file** (user's token/Path); (required)

```bash
$ fogbow-cli network --delete --auth-token mytoken --url http://localhost:8182

Ok
```

### Delete a single network
Delete a single network.

* **--delete** (required)
* **--url** (optional; default: http://localhost:8182): OCCI endpoint
* **--auth-token** (user's token/Text)  or **--auth-file** (user's token/Path); (required)
* **--id** (required): network id

```bash
$ fogbow-cli network --delete --auth-token mytoken --id networkid --url http://localhost:8182

Ok
```

### Create network
Create a network

* **--create** (required)
* **--url** (optional; default: http://localhost:8182): OCCI endpoint
* **--auth-token** (user's token/Text)  or **--auth-file** (user's token/Path); (required)
* **--cird** (required; format: ##.##.##.##/##) 
* **--gateway** (optional)
* **--allocation** (optional ;options: dynamic or static; default: dynamic)

```bash
$ fogbow-cli network --create --auth-token mytoken --url http://localhost:8182 --cird 10.10.10.0/24 --gateway 10.10.10.10 --allocation dynamic

X-OCCI-Location: federated_instance_230e00cd-4207-4d77-b40c-ba7ca8fa52fd
```

## Attachment operations (```attachment```)
### Get all attachment

Get all attachment associated to a particular user's token.

* **--get**  (required)
* **--url** (optional; default: http://localhost:8182): OCCI endpoint
* **--auth-token** (user's token/Text)  or **--auth-file** (user's token/Path); (required)

```bash
$ fogbow-cli attachment --get --auth-token mytoken --url http://localhost:10000

X-OCCI-Location: 687V5356-3432434-324324-3243242f
X-OCCI-Location: 09129582-8907667-123457-0765345c
```

### Get a single attachment
Get detailed information about a single attachment.

* **--get**  (required)
* **--url** (optional; default: http://localhost:8182): OCCI endpoint
* **--auth-token** (user's token/Text)  or **--auth-file** (user's token/Path); (required)
* **--id** (required): attachment id

Example: 
```bash
$ fogbow-cli attachment --get --auth-token mytoken --id networkid --url http://localhost:10000

occi.core.source=587V5356-3432434-324324-3243242f
occi.core.target=13029582-8907667-123457-0765345c
occi.core.id=E423CS82-8907667-123457-0765345c
occi.storagelink.provadingMemberId=provading_member
```

### Delete a single attachment
Delete a single attachment.

* **--delete** (required)
* **--url** (optional; default: http://localhost:8182): OCCI endpoint
* **--auth-token** (user's token/Text)  or **--auth-file** (user's token/Path); (required)
* **--id** (required): attachment id

```bash
$ fogbow-cli attachment --delete --auth-token mytoken --id networkid --url http://localhost:8182

Ok
```

### Create attachment 
Create a new attachment

* **--create** (required)
* **--url** (optional; default: http://localhost:8182): OCCI endpoint
* **--auth-token** (user's token/Text)  or **--auth-file** (user's token/Path); (required)
* **--computeId** (required)
* **--storageId** (required)

```bash
$ fogbow-cli attachment --create --auth-token mytoken --url http://localhost:8182 --computeId computeid --storageId storageid

X-OCCI-Location: http://locahost:8182/243029582-8907667-123457-0765345C@provading_member
```

## Accounting operations (```accounting```)
Get accounting

* **--url** (optional; default: http://localhost:8182): OCCI endpoint
* **--auth-token** (user's token/Text)  or **--auth-file** (user's token/Path); (required)

```bash
$ fogbow-cli accounting --auth-token mytoken --url http://localhost:8182

user; requesting_member; provading_member; 20
```
