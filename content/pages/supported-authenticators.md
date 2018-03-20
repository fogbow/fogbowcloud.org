Title: Supported Authenticators
url: supported-authenticators
save_as: supported-authenticators.html
section: usage
index: 2

Authentication Methods
==========

Here is a list containing all Authentication Methods currently supported by Fogbow-Cli. and the required parameters for creation

## Azure
* **--type azure** (optional): dynamic parameter

Parameters Required for creation:
* **--create** (required)
* **--type** (required) : identity plugin type
* **-Dkeystore_password=** (required): dynamic parameter
* **-Dsubscription_id=** (required): dynamic parameter
* **-Dkeystore_path=** (required): dynamic parameter

## CloudStack
* **--type cloudstack** (optional): dynamic parameter

Parameters Required for creation:
* **-DapiKey=** (required): dynamic parameter
* **-DsecretKey=** (required): dynamic parameter
* **-Dsignature=** (required): dynamic parameter

## EC2
* **--type ec2** (optional): dynamic parameter

Parameters Required for creation:
* **-DaccessKey=** (required): dynamic parameter
* **-DsecreKey=** (required): dynamic parameter

## LDAP
* **--type ldap** (optional): dynamic parameter

Parameters Required for creation:
* **-Dusername=** (required): dynamic parameter
* **-Dpassword=** (required): dynamic parameter
* **-Dbase=** (required): dynamic parameter
* **-DauthUrl=** (required): dynamic parameter
* **-DprivateKey=**   (required): dynamic parameter
* **-DpublicKey=** (required): dynamic parameter

## NAF
* **--type naf** (optional): dynamic parameter

Can't create NAF token

## Open Nebula
* **--type opennebula** (optional): dynamic parameter

Parameters Required for creation:
* **-Dusername=** (required): dynamic parameter
* **-Dpassword=** (required): dynamic parameter
Open Nebula identity_url must be configured on fogbow-manager

## Keystone V3
* **--type keystonev3** (optional): dynamic parameter

Parameters Required for creation:
* **-DauthUrl=** (required): dynamic parameter
* **-DprojectId=** (required): dynamic parameter
* **-Dpassword=** (required): dynamic parameter
* **-DuserId=** (required): dynamic parameter

## Keystone V2
* **--type keystonev2** (optional): dynamic parameter

Parameters Required for creation:
* **-DtenantName=** (required): dynamic parameter
* **-Dusername=** (required): dynamic parameter
* **-Dpassword=** (required): dynamic parameter

## SAML
* **--type saml** (optional): dynamic parameter

Can't create SAML token

## Shibboleth
* **--type shibboleth** (optional): dynamic parameter

Parameters Required for creation:
* **-DassertionKey=** (required): dynamic parameter
* **-DassertionId=** (required): dynamic parameter

## x509
* **--type x509** (optional): dynamic parameter

Parameters Required for creation:
* **-Dx509CertificatePath** (required): dynamic parameter

