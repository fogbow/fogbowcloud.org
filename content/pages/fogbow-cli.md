Title: Fogbow CLI
url: fogbow-cli
save_as: fogbow-cli.html
section: usage
index: 2

Fogbow CLI
==========

The Fogbow CLI is a command line interface for the Fogbow manager. It makes it easier for users to create HTTP requests and invoke them through the manager's REST API. All calls to the manager require a federation token. The Fogbow CLI provides a way for users to create such tokens. After creating a valid token, the users are able tovoke Fogbow CLI to get information about which cloud providers are available in the federation, what are their quota in these providers, and which images the providers make available to them; users can also create, retrive information and delete compute, network, and storage resources in any of the cloud providers available; finally, they can also and attach and detach volumes to compute resources.

## Installation
Follow these steps, <a  href="https://github.com/fogbow/fogbowcloud.org/blob/new-cli-documentation/content/pages/install-configure-fogbow-cli.md" target="_blank">to install and configure the Fogbow CLI</a>.

## Federation token operations (```token```)

### Note
It is necessary to create the identity plugin configuration file in your configuration path before executing token operations. An example for tokens provided by an LDAP service are shown below:

The ldap configuration file name must be named "ldap-identity-plugin.conf".
```bash
ldap_base = dc=site,dc=manager
ldap_identity_url = ldap://ldap.site.manager:389
ldap_encrypt_type = 
# the same configured in the fogbow manager
private_key_path = /home/ubuntu/private_key.pem
# the same configured in the fogbow manager
public_key_path = /home/ubuntu/public_key.pem
```

### Create a new Token
Create a new federation token.

Note: to pass the credentials it is necessary the use of dynamic parameters; follow the example with the **ldap** credentials:

* **--create** (required): operation
* **--type** (required): identity plugin type
* **--conf-path** (required): configuration path (you can ignore this parameter by setting FOGBOW_CONF_PATH as an environment variable). 
* **-Dpassword=** (required by ldap): dynamic parameter
* **-Dusername=** (required by ldap): dynamic parameter

Example:
```bash
$ fogbow-cli token --create --type ldap --conf-path /home/user/conf-folder -Dpassword=mypassword -Dusername=myuser

342dOiJjadsfsdfuYW1lIjoiRnJhbmNdsfsd35gZGdasdgdfbf2NRMzZlK3hJa0tSb3dZc3dkdmJ2K29SUVVpaWpCZkIwMG5JUlozdEZhQ2RPcXlEa2ZPZGFPWDIwM2lCYUIxZEVEbU43bjY0MXRlSkdIUm94OFdOWWNSbW9UlnlkIU873NJNBSDSJBAÇpHY01oWEpBVkhUbUV0Y01PbUoyV0JSUUorNUpVZWo4VWV0b2pDNGtrc0Nkb3lEU1MyVVJCNW5lVmNZNW9Wd29kOVB1UHZYNnQxRXN0KzU3eHdGdEdaTVlKZjR0eEI5Y2xaaHFzTDg3STJHNDJia3ByLzFGQ1lyM2x2eCt5a0twTEZPRHdoOXVVdUMyeWNtcExYOWIwamxDNnYzZWNEQThML3labndtNThnUU1TNXc1L1czd0VHWURUOGhjYWRnblVySjZSZzM4a0EA==
```

### Check token (```check-token```)
Check if token is valid.

* **--type** (required): identity plugin type
* **--conf-path** (required): configuration path (you can ignore this parameter by setting FOGBOW_CONF_PATH as an environment variable)
* **--federation-token-value** (required): federation token

Example:
```bash
$ fogbow-cli check-token --conf-path /home/user/conf-folder --type ldap --federation-token-value my-token-value

Token Valid
```

## User operations (```user```)

### Show user information 
Get the user id and attributes associated to a particular token.

* **--get-user** (required): operation
* **--type** (required): identity plugin type
* **--conf-path** (required): configuration path (you can ignore this parameter by setting FOGBOW_CONF_PATH as na environment variable)
* **--federation-token-value** (required): federation token

Example:
```bash
$ fogbow-cli user --get-user --conf-path /home/user/conf-folder --type ldap --federation-token-value my-token-value

{"id":"fogbow","attributes":{"user-name":"Fogbow User"}}
```

## Compute operations (```compute```)

### Create compute
Create a compute.

* **--create** (required): operation
* **--federation-token-value** (required): federation token
* **--url** (required): url of the manager 
* **--providing-member** (required): member that will provide the compute
* **--public-key** (optional): user's public-key
* **--image-id** (required): image
* **--vcpu** (required): number of cores 
* **--memory** (required): in MB's
* **--disk** (required): in GB's

Example:
```bash
$ fogbow-cli compute --create  --url manager-url --federation-token-value my-token-value --providing-member providing-member-url --public-key public-key-path --image-id image-id --vcpu vcpu --memory ram --disk disk

{"id": "compute-id"}  
```

### Get all computes
Get all computes associated to a particular federation token.

* **--get-all** (required): operation 
* **--federation-token-value** (required): federation token
* **--url** (required): url of the manager 

Example:
```bash
$ fogbow-cli compute --get-all --url manager-url --federation-token-value my-token-value

[{"vCPU": 10, "hostName": "hostName", "localIpAddress": "localIpAddress", "state": "READY", "memory": 10, "sshTunnelConnectionData": {"sshUserName": "fogbowuser", "sshPublicAddress": "10.10.0.120", "sshExtraPorts": "80"}, "id": "v2"}, ...]
```

### Get a single compute
Get detailed information about a single compute.

* **--get** (required): operation
* **--federation-token-value** (required): federation token
* **--url** (required): url of the manager
* **--id** (required): compute id

Example:
```bash
$ fogbow-cli compute --get --id compute-id --url manager-url --federation-token-value my-token-value

{"vCPU": 10, "hostName": "hostName", "localIpAddress": "localIpAddress", "state": "READY", "memory": 10, "sshTunnelConnectionData": {"sshUserName": "fogbesdras", "sshPublicAddress": "10.10.0.120", "sshExtraPorts": "80"}, "id": "v2"}
```

### Delete a single compute
Delete a single compute.

* **--delete**: operation
* **--federation-token-value** (required): federation token
* **--url** (required): url of the manager
* **--id** (required):  compute id

Example:
```bash
$ fogbow-cli compute --delete --id instance-id --url manager-url --federation-token-value my-token-value

Ok
```

### Get compute quota
Get cloud quota information of the federation member

* **--get-quota**: operation
* **--federation-token-value** (required): federation token
* **--url** (required): url of the manager
* **--member-id** (required): federation member

```bash
$ fogbow-cli compute --get-allocation --member-id member --url manager-url --federation-token-value my-token-value

{"totalQuota": {"vCPU": "1", "ram": "2", "instances": "1"}, "usedQuota": {"vCPU": "1", "ram": "2", "instances": "1"}}
```

### Get compute allocation
Get user allocation information of the federation member
* **--get-allocation**: operation
* **--federation-token-value** (required): federation token
* **--url** (required): url of the manager
* **--member-id** (required): federation member

```bash
$ fogbow-cli compute --get-allocation --member-id member --url manager-url --federation-token-value my-token-value

{"vCPU": "1", "ram": "2", "instances": "1"}
```

## Network operations (```network```)

### Create network
Create a network.

* **--create** (required): operation
* **--url** (required): url of the manager 
* **--federation-token-value** (required): federation token
* **--providing-member** (required): member that will provide the
* **--address** (required; format: ##.##.##.##/##): cird
* **--gateway** (optional)
* **--allocation** (optional; options: dynamic or static; default: dynamic)

Example:
```bash
$ fogbow-cli network --create  --url manager-url --federation-token-value my-token-value --address cidr --gateway gateway-ip --allocation allocation --providing-member providing-member

{"id": "network-id"} 
```

### Get all networks
Get all networks associated to a particular federation token.

* **--get-all**(required): operation 
* **--federation-token-value** (required): federation token
* **--url** (required): url of the manager 

Example:
```bash
$ fogbow-cli network --get-all --url manager-url --federation-token-value my-token-value

[{"id": "id", "label": "label", "state": "state", "address": "address", "gateway": "gateway", "vLAN": "vLAN", "networkAllocation": {"value": "value"}, "networkInterface": "networkInterface", "MACInterface": "MACInterface", "interfaceState": "interfaceState"}, ...]
```

### Get a single network
Get detailed information about a single network.

* **--get**(required): operation 
* **--federation-token-value** (required): federation token
* **--url** (required): url of the manager
* **--id** (required):  network id

Example:
```bash
$ fogbow-cli network --get --id network-id --url manager-url --federation-token-value my-token-value

{"id": "id", "label": "label", "state": "state", "address": "address", "gateway": "gateway", "vLAN": "vLAN", "networkAllocation": {"value": "value"}, "networkInterface": "networkInterface", "MACInterface": "MACInterface", "interfaceState": "interfaceState"}
```

### Delete a single network
Delete a single network.

* **--delete**(required): operation 
* **--federation-token-value** (required): federation token
* **--url** (required): url of the manager
* **--id** (required):  network id

Example:
```bash
$ fogbow-cli network --delete --id instance-id --url manager-url --federation-token-value my-token-value

Ok
```

## Volume operations (```volume```)

### Create volume
Create a volume.

* **--create** (required): operation
* **--url** (required): url of the manager 
* **--federation-token-value** (required): federation token
* **--providing-member** (required): member that will provide the volume
* **--volume-size** (required; Unit GB)

Example:
```bash
$ fogbow-cli volume --create  --url manager-url --federation-token-value my-token-value --providing-member providing-member --volume-size size

{"id": "volume-id"} 
```

### Get all volumes
Get all volumes associated to a particular federation token.

* **--get-all** (required): operation
* **--federation-token-value** (required): federation token
* **--url** (required): url of the manager 

Example:
```bash
$ fogbow-cli volume --get-all --url manager-url --federation-token-value my-token-value

[{"name": "name_one", "size": "1"}, {"name": "name_two", "size": "1"}]
```

### Get a single volume
Get detailed information about a single volume.

* **--get** (required): operation
* **--federation-token-value** (required): federation token
* **--url** (required): url of the manager
* **--id** (required):  volume id

Example:
```bash
$ fogbow-cli volume --get --id volume-id --url manager-url --federation-token-value my-token-value

{"name": "name", "size": "1"}
```

### Delete a single volume
Delete a single volume.

* **--delete** (required): operation
* **--federation-token-value** (required): federation token
* **--url** (required): url of the manager
* **--id** (required):  volume id

Example:
```bash
$ fogbow-cli volume --delete --id volume-id --url manager-url --federation-token-value my-token-value

Ok
```

## Attachment operations (```attachment```)

### Create attachment
Create an attachment.

* **--create** (required): operation
* **--url** (required): url of the manager 
* **--federation-token-value** (required): federation token
* **--providing-member** (required): member that will provide the attachment
* **--source** (required): compute id
* **--target** (required): volume id
* **--device** (optional): device's name in the virtual machine. 

Example:
```bash
$ fogbow-cli attachment --create  --url manager-url --federation-token-value my-token-value --providing-member providing-member --source source --target target --device device

{"id": "attachment-id"}
```

### Get all attachments
Get all attachments associated to a particular federation token.

* **--get-all** (required): operation
* **--federation-token-value** (required): federation token
* **--url** (required): url of the manager 

Example:
```bash
$ fogbow-cli attachment --get-all --url manager-url --federation-token-value my-token-value

[{"serverId": "serverId", "volumeId": "volumeId", "device": "device"}, {"serverId": "serverId", "volumeId": "volumeId", "device": "device"}]
```

### Get a single attachment
Get detailed information about a single attachment.

* **--get** (required): operation
* **--federation-token-value** (required): federation token
* **--url** (required): url of the manager
* **--id** (required):  attachment id

Example:
```bash
$ fogbow-cli attachment --get --id attachment-id --url manager-url --federation-token-value my-token-value

{"serverId": "serverId", "volumeId": "volumeId", "device": "device"}
```

### Delete a single attachment
Delete a single attachment.

* **--get** (required): operation
* **--federation-token-value** (required): federation token
* **--url** (required): url of the manager
* **--id** (required):  attachment id

Example:
```bash
$ fogbow-cli attachment --delete --id attachment-id --url manager-url --federation-token-value my-token-value

Ok
```

## Image operations (```image```)

### Get all images
Get all images associated to a particular member.

* **--get** (required): operation
* **--federation-token-value** (required): federation token
* **--url** (required): url of the manager
* **--member-id** (required): federation member

```bash
$ fogbow-cli image --get-all --url manager-url --federation-token-value my-token-value --member-id member

{"000fdc22-e5f5-4c9a-98f8-998dab72f9bf": "ubuntu-16.04-Nov-17", "123f7485-92d5-4533-a329-0dbaca22ae4e": "debian-8-Oct-17", "3211fa8f-aa38-4fa7-8ecb-3b117d98d186": "centos-Sep-17"}
```

### Get a single image
Get detailed information about a single image.

* **--get** (required): operation
* **--federation-token-value** (required): federation token
* **--url** (required): url of the manager
* **--member-id** (required): federation member
* **--id** (required): image id

```bash
$ fogbow-cli image --get --url manager-url --federation-token-value my-token-value --member-id member --id image-id

{"id":"id", "name":"name", "size":"size", "minDisk":"2", "minRam": "2", "status":"status"}
```

## Federation member operations (```member```)

### Get federation members
* **--get-all** (required): operation
* **--url** (required): url of the rendezvous

```bash
$ fogbow-cli member --get-all --url rendezvous-url

["member1", "member2", "member3"]
```
