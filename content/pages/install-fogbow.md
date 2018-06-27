Title: Install Fogbow Infrastructure
url: install-fogbow-infra
save_as: install-fogbow-infra.html
section: install
index: 7

Install Fogbow Infrastructure
==========
This tutorial provides a easy way to deploy all fogbow infrastructure.

## Pre-Installation

Antes de realizar a instalação é necessário fazer algumas configurações a nível administrativo.

### Hosts

É necessário possuir dois hosts o **dmz-host** e **internal-host**. Pelo menos o **dmz-host** precisa ter um IP público associado.

### Firewall configuration

O **dmz-host** deve ficar na DMZ (Demilitarized Zone) com as seguintes portas liberadas:

1. XMPP server to server communication port (**Default**: 5327);
2. Reverse tunnel ssh ports range;
3. Reverse tunnel external services ports range.

### DNS configuration

É necessário configurar o DNS para que a partir do **XMPP ID** a.k.a **member-site-id** seja possível se comunicar com o **dmz-host** via troca de mensagens XMPP. Ou seja, é necessário associar o **XMPP ID** ao IP público do **dmz-host**.
