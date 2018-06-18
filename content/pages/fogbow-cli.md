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
* **--url** (required)
* **--federation-token-value** (required)
* **--providing-member** (required)
* **--public-key** (optional)
* **--image-id** (required)
* **--vcpu** (required): number of cores 
* **--memory** (required): in MB's
* **--disk** (required): in GB's

```bash
$ fogbow-cli compute --create --federation-token-value my-token-value --providing-member providing-member-url 

X-OCCI-Location: http://localhost:8182/federated_instance_230e00cd-4207-4d77-b40c-ba7ca8fa52fd
```

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
$ fogbow-cli instance --get --auth-token mytoken --id instanceid --url http://localhost:8182

Category: compute; scheme="http://schemas.ogf.org/occi/infrastructure#"; class="kind"; title="Compute Resource"; rel="http://schemas.ogf.org/occi/core#resource"; location="http://localhost:8182/compute/"; attributes="occi.compute.architecture occi.compute.state{immutable} occi.compute.speed occi.compute.memory occi.compute.cores occi.compute.hostname"; actions="http://schemas.ogf.org/occi/infrastructure/compute/action#start http://schemas.ogf.org/occi/infrastructure/compute/action#stop http://schemas.ogf.org/occi/infrastructure/compute/action#restart http://schemas.ogf.org/occi/infrastructure/compute/action#suspend"
Category: os_tpl; scheme="http://schemas.ogf.org/occi/infrastructure#"; class="mixin"; location="http://localhost:8182/os_tpl/"
Category: flavor.small; scheme="http://schemas.fogbowcloud.org/template/resource#"; class="mixin"; title="flavor.small"; rel="http://schemas.ogf.org/occi/infrastructure#resource_tpl"; location="http://localhost:8182/flavor.small/"
Category: fogbow-image; scheme="http://schemas.fogbowcloud.org/template/os#"; class="mixin"; title="fogbow-image image"; rel="http://schemas.ogf.org/occi/infrastructure#os_tpl"; location="http://localhost:8182/fogbow-image/"
X-OCCI-Attribute: occi.compute.state="active"
X-OCCI-Attribute: occi.compute.hostname="fogbow-instance-230e00cd-4207-4d77-b40c-ba7ca8fa52fd"
X-OCCI-Attribute: org.fogbowcloud.order.ssh-username="fogbow"
X-OCCI-Attribute: occi.compute.memory="2.0"
X-OCCI-Attribute: org.fogbowcloud.order.local-ip-address="0.0.0.0"
X-OCCI-Attribute: org.fogbowcloud.order.extra-ports="{"postgres":"0.0.0.0:10029"}"
X-OCCI-Attribute: occi.compute.cores="1"
X-OCCI-Attribute: org.fogbowcloud.order.ssh-public-address="0.0.0.0:10028"
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
