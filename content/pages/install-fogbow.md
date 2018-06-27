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

2. Install [ansible](https://docs.ansible.com/ansible/latest/installation_guide/intro_installation.html).

### Fogbow configuration

Go to the directory *conf-files* inside *fogbow-playbook* directory.

```bash
cd fogbow-playbook/conf-files
```

After it is necessary to edit 11 small configuration files. Please, note that is only **necessary** to edit the configuration constants that the line above contains the token **"*# Required*".**

#### behavior.conf

