Title: Install Fogbow Infrastructure
url: install-fogbow-infra
save_as: install-fogbow-infra.html
section: install
index: 7

Install fogbow infrastructure
==========
This tutorial provides a easy way to deploy the entire fogbow infrastructure.

## Pre-installation

Antes de realizar a instalação é necessário fazer algumas configurações a nível administrativo.

### Hosts

É necessário possuir dois hosts o **dmz-host** e **internal-host**. Pelo menos o **dmz-host** precisa ter um IP público associado.

### Firewall configuration

O **dmz-host** deve ficar na DMZ (Demilitarized Zone) com as seguintes portas liberadas:

1. XMPP server to server communication port (**Default**: *5327*);
2. Reverse tunnel ssh ports range;
3. Reverse tunnel external services ports range.

### DNS configuration

É necessário configurar o DNS para que a partir do **XMPP ID** a.k.a **member-site-id** seja possível se comunicar com o **dmz-host** via troca de mensagens XMPP. Ou seja, é necessário associar o **XMPP ID** ao IP público do **dmz-host**.

### Cloud configuration

É necessário criar um usuário para que seja possível o fogbow middleware criar e administrar recursos na cloud.

## Installation

### Environment setup

1. Download the *fogbow-playbook* project:

```bash
git clone https://github.com/fogbow/fogbow-playbook.git
```

2. Install [Ansible](https://docs.ansible.com/ansible/latest/installation_guide/intro_installation.html).

### Fogbow configuration

Go to the directory *conf-files* inside *fogbow-playbook* directory.

```bash
cd fogbow-playbook/conf-files
```

After it is necessary to edit 11 configuration files. Please, note that is only **necessary** to edit the configuration constants that the line above contains the token **"*# Required*".**

#### Hosts configuration

File: [hosts.conf](https://github.com/fogbow/fogbow-playbook/blob/master/conf-files/hosts.conf)

The ***dmz_host_private_ip*** configuration constant is the **dmz-host** private network address.

The ***dmz_host_public_ip*** configuration constant is the **dmz-host** public network address.

The ***internal_host_private_ip*** configuration constant is the **internal-host** private network address.

#### Behavior configuration

File: [behavior.conf](https://github.com/fogbow/fogbow-playbook/blob/master/conf-files/behavior.conf)

To know more about the ***behavior.conf*** constants please see [please-give-me-an-explanation-link](http://www.fogbowcloud.org).

After the **behavior.conf** edition is necessary to edit the federation identity, authorization and local user credentials mapper configuration files that were configured in the **behavior.conf**.

#### Local user credentials mapper 

See the local user credentials mapper configuration files list [here](https://github.com/fogbow/fogbow-playbook/tree/master/conf-files/behavior-plugins/local-user-credentials-mapper). Please, configure the local user credentials mapper used in the **behavior.conf**.

##### Default

File: [default_mapper.conf](https://github.com/fogbow/fogbow-playbook/blob/master/conf-files/behavior-plugins/local-user-credentials-mapper/default_mapper.conf)

To know more about the ***default_mapper.conf*** constants please see [please-give-me-an-explanation-link](http://www.fogbowcloud.org).

#### Federation identity 

See the federation identity configuration files list [here](https://github.com/fogbow/fogbow-playbook/tree/master/conf-files/behavior-plugins/federation-identity). Please, configure the federation identity used in the **behavior.conf**.

##### LDAP

File: [ldap-identity-plugin.conf](https://github.com/fogbow/fogbow-playbook/blob/master/conf-files/behavior-plugins/federation-identity/ldap-identity-plugin.conf)

To know more about the ***ldap-identity-plugin.conf*** constants please see [please-give-me-an-explanation-link](http://www.fogbowcloud.org).

##### Default

Configuration is not necessary.

#### Cloud configuration

File: [cloud.conf](https://github.com/fogbow/fogbow-playbook/blob/master/conf-files/cloud.conf)

To know more about the ***cloud.conf*** constants please see [please-give-me-an-explanation-link](http://www.fogbowcloud.org).

After the **cloud.conf** edition is necessary to edit the cloud type configuration file that was configured in the **cloud.conf**, see the cloud types configuration files list [here](https://github.com/fogbow/fogbow-playbook/tree/master/conf-files/cloud-plugins).

##### OpenStack

File: [openstack.conf](https://github.com/fogbow/fogbow-playbook/blob/master/conf-files/cloud-plugins/openstack.conf)

To know more about the ***openstack.conf*** constants please see [please-give-me-an-explanation-link](http://www.fogbowcloud.org).

#### Manager configuration

File: [manager.conf](https://github.com/fogbow/fogbow-playbook/blob/master/conf-files/manager.conf)

The ***server_port*** configuration constant is the port that the Fogbow Manager component will server requests in the **internal-host**.

The ***manager_ssh_public_key_file_path*** and ***manager_ssh_private_key_file_path*** configuration constants are not required, however if they are not configured the *fogbow-playbook* will generate the keys automatically placing them at the *fogbow-playbook* directory.

To know more about the ***manager.conf*** constants please see [please-give-me-an-explanation-link](http://www.fogbowcloud.org).

#### Intercomponent configuration

File: [intercomponent.conf](https://github.com/fogbow/fogbow-playbook/blob/master/conf-files/intercomponent.conf)

To know more about the ***intercomponent.conf*** constants please see [please-give-me-an-explanation-link](http://www.fogbowcloud.org).

#### Membership configuration

File: [membership.conf](https://github.com/fogbow/fogbow-playbook/blob/master/conf-files/membership.conf)

The ***server_port*** configuration constant is the port that the Membership component will server requests in the **internal-host**, note that the Membership service ***server_port*** should be different of the Manager ***server_port***.

To know more about the ***membership.conf*** constants please see [please-give-me-an-explanation-link](http://www.fogbowcloud.org).

#### Reverse-tunnel configuration

File: [reverse-tunnel.conf](https://github.com/fogbow/fogbow-playbook/blob/master/conf-files/reverse-tunnel.conf)

The ***host_key_path*** configuration constant is not required, however if it is not configured the *fogbow-playbook* will use the ***manager_ssh_private_key_file_path***.

For the configuration constants ***reverse_tunnel_port*** and ***reverse_tunnel_http_port*** do not choose the 80 port because this port is used to server the Dashboard front-end in the **dmz-host**.

To know more about the ***reverse-tunnel.conf*** constants please see [please-give-me-an-explanation-link](http://www.fogbowcloud.org).
