Title: The Big Picture
url: the-big-picture
save_as: the-big-picture.html
section: install
index: 0

# The Big Picture

## Deployment infrastructure

A typical deployment of Fogbow will require two dedicated machines (either real or virtual). Let us call them **internal.mydomain** and **external.mydomain**. The **internal.mydomain** machine is part of the organization's private network and must have access to the endpoints of the cloud that is being federated. On the other hand, the **external.mydomain** machine should be outside the organization's private network, within a DMZ.

The Messaging Service (MS) is deployed in **external.mydomain**, while the Resource Allocation Service (RAS) is deployed in **internal.mydomain**. Usually, the latter is also the machine where all other optional Fogbow services are deployed.  in the site will usually be deployed in  and FM components are deployed in the **internal.mydomain** machine, while the XMPP server and the FRT component are deployed in the **external.mydomain** machine.
