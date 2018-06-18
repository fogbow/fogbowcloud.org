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

## Token operations (```token```)

### Create a new Token
Create a new user token.

Note: to pass the credentials it is necessary the use of dynamic parameters; follow the example with the **OpenStack** credentials:

* **--create** (required)
* **--type** (required) : identity plugin type
* **--conf-path** (required): identity plugin configuration file path (you can ignore this parameter by setting FOGBOW_CONF_PATH as a environment variable)
* **-Dpassword=** (optional): dynamic parameter
* **-Dusername=** (optional): dynamic parameter

Example:
```bash
token --create --type ldap --conf-path /home/user/conf-folder -Dpassword=mypassword -Dusername=myuser
```

Others credentails can be found on the Supported Authentication Methods document [link]:

### Check token
Check if token is valid.

* **--type** (required): identity plugin type
* **--conf-path** (required): identity plugin configuration file path (you can ignore this parameter by setting FOGBOW_CONF_PATH as a environment variable)
* **--federation-token-value** (required) : user's token

Example:
```bash
$ fogbow-cli check-token --conf-path /home/user/conf-folder --type ldap --federation-token-value my-token-value
```

## User operations (```user```)

### Show user information 
Get the member id and attributes associated to a particular user's token.

* **--get-user** (required)
* **--type** (required): identity plugin type
* **--conf-path** (required): identity plugin configuration file path (you can ignore this parameter by setting FOGBOW_CONF_PATH as a environment variable)
* **--federation-token-value** (required) : user's token

Example:
```bash
$ fogbow-cli user --get-user --conf-path /home/user/conf-folder --type ldap --federation-token-value my-token-value

{"id":"fogbow","attributes":{"user-name":"Fogbow User"}}
```

## Compute operations (```instance```)

### Create compute
Create a compute.

* **--create** (required)
* **--federation-token-value** (required)
* **--providing-member** (required)
* **--public-key** (optional)
* **--image-id** (required)
* **--vcpu** (required): number of cores 
* **--url** (required): url of the manager 
* **--memory** (required): in MB's
* **--disk** (required): in GB's

Example:
```bash
$ fogbow-cli compute --create  --url manager-url --federation-token-value my-token-value --providing-member providing-member-url --public-key public-key-path --image-id image-id --vcpu vcpu --memory ram --disk disk

{"id": "compute-id"}  
```

### Get all instances
Get all instances associated to a particular user's token.

* **--federation-token-value** (required)
* **--url** (required): url of the manager 

Example:
```bash
$ fogbow-cli compute --get-all --url manager-url --federation-token-value my-token-value

[{"vCPU": 10, "hostName": "hostName", "localIpAddress": "localIpAddress", "state": "READY", "memory": 10, "sshTunnelConnectionData": {"sshUserName": "fogbesdras", "sshPublicAddress": "10.10.0.120", "sshExtraPorts": "80"}, "id": "v2"}, ...]
```

### Get a single instance
Get detailed information about a single instance.

* **--federation-token-value** (required)
* **--url** (required): url of the manager
* **--id** (required):  instance id

Example:
```bash
$ fogbow-cli compute --get --id instance-id --url manager-url --federation-token-value my-token-value

{"vCPU": 10, "hostName": "hostName", "localIpAddress": "localIpAddress", "state": "READY", "memory": 10, "sshTunnelConnectionData": {"sshUserName": "fogbesdras", "sshPublicAddress": "10.10.0.120", "sshExtraPorts": "80"}, "id": "v2"}
```

### Delete a single instance
Delete a single instance.

* **--federation-token-value** (required)
* **--url** (required): url of the manager
* **--id** (required):  instance id

Example:
```bash
$ fogbow-cli compute --delete --id instance-id --url manager-url --federation-token-value my-token-value

Ok
```

## Network operations (```network```)

### Create network
Create a network.

* **--create** (required)
* **--url** (required): url of the manager 
* **--federation-token-value** (required)
* **--address** (required; format: ##.##.##.##/##): cird
* **--gateway** (optional)
* **--allocation** (optional; options: dynamic or static; default: dynamic)

Example:
```bash
$ fogbow-cli network --create  --url manager-url --federation-token-value my-token-value --address cidr --gateway gateway-ip --allocation allocation

{"id": "network-id"} 
```

### Get all networks
Get all networks associated to a particular user's token.

* **--federation-token-value** (required)
* **--url** (required): url of the manager 

Example:
```bash
$ fogbow-cli network --get-all --url manager-url --federation-token-value my-token-value

[{"id": "id", "label": "label", "state": "state", "address": "address", "gateway": "gateway", "vLAN": "vLAN", "networkAllocation": {"value": "value"}, "networkInterface": "networkInterface", "MACInterface": "MACInterface", "interfaceState": "interfaceState"}, ...]
```

### Get a single network
Get detailed information about a single network.

* **--federation-token-value** (required)
* **--url** (required): url of the manager
* **--id** (required):  network id

Example:
```bash
$ fogbow-cli network --get --id network-id --url manager-url --federation-token-value my-token-value

{"id": "id", "label": "label", "state": "state", "address": "address", "gateway": "gateway", "vLAN": "vLAN", "networkAllocation": {"value": "value"}, "networkInterface": "networkInterface", "MACInterface": "MACInterface", "interfaceState": "interfaceState"}
```

### Delete a single network
Delete a single network.

* **--federation-token-value** (required)
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

* **--create** (required)
* **--url** (required): url of the manager 
* **--federation-token-value** (required)
* **--providing-member** (required)
* **--volume-size** (required; Unit GB)

Example:
```bash
$ fogbow-cli network --create  --url manager-url --federation-token-value my-token-value --providing-member providing-member --volume-size size

{"id": "volume-id"} 
```

### Get all volumes
Get all volumes associated to a particular user's token.

* **--federation-token-value** (required)
* **--url** (required): url of the manager 

Example:
```bash
$ fogbow-cli network --get-all --url manager-url --federation-token-value my-token-value
```

### Get a single volume
Get detailed information about a single volume.

* **--federation-token-value** (required)
* **--url** (required): url of the manager
* **--id** (required):  volume id

Example:
```bash
$ fogbow-cli volume --get --id volume-id --url manager-url --federation-token-value my-token-value
```

### Delete a single volume
Delete a single volume.

* **--federation-token-value** (required)
* **--url** (required): url of the manager
* **--id** (required):  volume id

Example:
```bash
$ fogbow-cli volume --delete --id volume-id --url manager-url --federation-token-value my-token-value

Ok
```
