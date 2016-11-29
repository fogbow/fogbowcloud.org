Deployment Test
======

1. List federation members

```shell
./bin/fogbow-cli member --url http://localhost:8182 --auth-token $token

tu.dresden.manager.naf.lsd.ufcg.edu.br
lsd.manager.naf.lsd.ufcg.edu.br
azure.manager.naf.lsd.ufcg.edu.br
```

2. See member quota
```shell
./bin/fogbow-cli member --url http://localhost:8182 --quota --id tu.dresden.manager.naf.lsd.ufcg.edu.br --auth-token $token

X-OCCI-Attribute: cpuQuota=20
X-OCCI-Attribute: cpuInUse=0
X-OCCI-Attribute: cpuInUseByUser=0
X-OCCI-Attribute: memQuota=51200
X-OCCI-Attribute: memInUse=0
X-OCCI-Attribute: memInUseByUser=0
X-OCCI-Attribute: instancesQuota=10
X-OCCI-Attribute: instancesInUse=0
X-OCCI-Attribute: instancesInUseByUser=0
```

3. Create Compute Order.
```shell
./bin/fogbow-cli order --create --n 1 --image fogbow-ubuntu --url http://localhost:8182 --public-key /home/ubuntu/fogbow-instalation/keys/chico.pub --requirements "Glue2RAM >= 1024" --resource-kind compute --auth-token $token

X-OCCI-Location: http://10.7.41.12:8182/order/65a746b4-4b84-455a-b36a-bf64f030ab56
```

4. List Order.
```shell
./bin/fogbow-cli order --get --auth-token $token --url http://localhost:8182

X-OCCI-Location: http://10.7.41.12:8182/order/65a746b4-4b84-455a-b36a-bf64f030ab56
```

5. Get order’s (65a746b4-4b84-455a-b36a-bf64f030ab56) details and check if it is FULFILLED.
```shell
./bin/fogbow-cli order --get --auth-token $token --url http://localhost:8182 --id 65a746b4-4b84-455a-b36a-bf64f030ab56

[...]
X-OCCI-Attribute: org.fogbowcloud.order.state="fulfilled"
[...]
X-OCCI-Attribute: org.fogbowcloud.order.instance-id="f32eb79d-5681-488a-a6e3-fd0fe8426b96@tu.dresden.manager.naf.lsd.ufcg.edu.br" 
[...]
```

6. See last instance’s(f32eb79d-5681-488a-a6e3-fd0fe8426b96@tu.dresden.manager.naf.lsd.ufcg.edu.br) details
```shell
./bin/fogbow-cli instance --get --auth-token $token --url http://localhost:8182 --id f32eb79d-5681-488a-a6e3-fd0fe8426b96@tu.dresden.manager.naf.lsd.ufcg.edu.br

[...]
X-OCCI-Attribute: occi.core.id="f32eb79d-5681-488a-a6e3-fd0fe8426b96"
X-OCCI-Attribute: org.fogbowcloud.order.ssh-public-address="141.76.45.4:23451"
[...]
```

7. Try to access by SSH and check it is ok.
```shell
ssh fogbow@141.76.45.4 - p 23451
```

8. Create Storage
```shell
./bin/fogbow-cli order --create --n 1 --resource-kind storage --url http://localhost:8182 --size 1 --auth-token $token

X-OCCI-Location: http://10.7.41.12:8182/order/837a3124-b871-47c7-8305-613e4d1b100f
```

9. Get order’s(837a3124-b871-47c7-8305-613e4d1b100f) detail and check if is FULFILLED.
```shell
./bin/fogbow-cli order --get --auth-token $token --url http://localhost:8182 --id 837a3124-b871-47c7-8305-613e4d1b100f

[...]
X-OCCI-Attribute: org.fogbowcloud.order.state="fulfilled" 
[...]
X-OCCI-Attribute: org.fogbowcloud.order.instance-id="ef01b2b6-ab77-4705-92a2-83642d88e2ef@tu.dresden.manager.naf.lsd.ufcg.edu.br" 
[...]
```

Create attachment with compute(f32eb79d-5681-488a-a6e3-fd0fe8426b96@tu.dresden.manager.naf.lsd.ufcg.edu.br) and storage(ef01b2b6-ab77-4705-92a2-83642d88e2ef@tu.dresden.manager.naf.lsd.ufcg.edu.br).
./bin/fogbow-cli attachment --create --auth-token $token --url http://localhost:8182 --computeId f32eb79d-5681-488a-a6e3-fd0fe8426b96@tu.dresden.manager.naf.lsd.ufcg.edu.br --storageId ef01b2b6-ab77-4705-92a2-83642d88e2ef@tu.dresden.manager.naf.lsd.ufcg.edu.br
http://10.7.41.12:8182/storage/link//ef01b2b6-ab77-4705-92a2-83642d88e2ef@tu.dresden.manager.naf.lsd.ufcg.edu.br


Get attachment’s(ef01b2b6-ab77-4705-92a2-83642d88e2ef@tu.dresden.manager.naf.lsd.ufcg.edu.br) details
./bin/fogbow-cli attachment --get --auth-token $token --id ef01b2b6-ab77-4705-92a2-83642d88e2ef@tu.dresden.manager.naf.lsd.ufcg.edu.br --url http://localhost:8182
storagelink; scheme="http://schemas.ogf.org/occi/infrastructure#"; class="kind"; title="A link to a storage resource"; rel="http://schemas.ogf.org/occi/core#link"; location="http://localhost:8182/storage/link/"; attributes="occi.storagelink.state{immutable} occi.storagelink.mountpoint occi.storagelink.deviceid"
X-OCCI-Attribute: occi.storagelink.provadingMemberId="tu.dresden.manager.naf.lsd.ufcg.edu.br"
X-OCCI-Attribute: occi.storagelink.deviceid="null"
X-OCCI-Attribute: occi.core.target="ef01b2b6-ab77-4705-92a2-83642d88e2ef"
X-OCCI-Attribute: occi.core.source="f32eb79d-5681-488a-a6e3-fd0fe8426b96"


Delete attachment(ef01b2b6-ab77-4705-92a2-83642d88e2ef@tu.dresden.manager.naf.lsd.ufcg.edu.br)
./bin/fogbow-cli attachment --delete --auth-token $token --url http://localhost:8182 --id ef01b2b6-ab77-4705-92a2-83642d88e2ef@tu.dresden.manager.naf.lsd.ufcg.edu.br
Ok


Delete instance (f32eb79d-5681-488a-a6e3-fd0fe8426b96@tu.dresden.manager.naf.lsd.ufcg.edu.br).
./bin/fogbow-cli instance --delete --auth-token $token --url http://localhost:8182 --id f32eb79d-5681-488a-a6e3-fd0fe8426b96@tu.dresden.manager.naf.lsd.ufcg.edu.br
Ok


Delete order (65a746b4-4b84-455a-b36a-bf64f030ab56).
./bin/fogbow-cli order --delete --auth-token $token --url http://localhost:8182 --id 65a746b4-4b84-455a-b36a-bf64f030ab56
Ok


Delete storage(ef01b2b6-ab77-4705-92a2-83642d88e2ef@tu.dresden.manager.naf.lsd.ufcg.edu.br)
./bin/fogbow-cli storage --delete --auth-token $token --url http://localhost:8182 --id ef01b2b6-ab77-4705-92a2-83642d88e2ef@tu.dresden.manager.naf.lsd.ufcg.edu.br
Ok


Create order network
./bin/fogbow-cli order --create --n 1 --resource-kind network --cidr 172.16.0.0/12 --gateway 172.16.0.0 --allocation dynamic --url http://localhost:8182 --auth-token $token
X-OCCI-Location: http://10.7.41.12:8182/order/0ee0578b-38ed-4d77-98c7-cc4c51f57c2d


Get order’s (0ee0578b-38ed-4d77-98c7-cc4c51f57c2d) details and check if it is FULFILLED.
./bin/fogbow-cli order --get --auth-token $token --url http://localhost:8182 --id 0ee0578b-38ed-4d77-98c7-cc4c51f57c2d
Category: order; scheme="http://schemas.fogbowcloud.org/order#"; class="kind"; title="Order new Instances"; rel="http://schemas.ogf.org/occi/core#resource"; location="http://localhost:8182/order/"; attributes="org.fogbowcloud.order.instance-count org.fogbowcloud.order.type org.fogbowcloud.order.valid-until org.fogbowcloud.order.valid-from org.fogbowcloud.order.state org.fogbowcloud.order.instance-id org.fogbowcloud.credentials.publickey.data org.fogbowcloud.order.user-data org.fogbowcloud.order.extra-user-data org.fogbowcloud.order.extra-user-data-content-type org.fogbowcloud.order.requirements org.fogbowcloud.order.batch-id org.fogbowcloud.order.requesting-member org.fogbowcloud.order.providing-member org.fogbowcloud.order.resource-kind org.fogbowcloud.order.storage-size org.fogbowcloud.order.network-id"
X-OCCI-Attribute: occi.network.label="Not defined" 
X-OCCI-Attribute: org.fogbowcloud.order.network-id="Network default" 
X-OCCI-Attribute: org.fogbowcloud.order.requesting-member="tu.dresden.manager.naf.lsd.ufcg.edu.br" 
X-OCCI-Attribute: org.fogbowcloud.order.state="fulfilled" 
X-OCCI-Attribute: org.fogbowcloud.order.extra-user-data="Not defined" 
X-OCCI-Attribute: org.fogbowcloud.order.batch-id="7afacf11-3ee0-4388-add2-c25be58554a4" 
X-OCCI-Attribute: org.fogbowcloud.order.valid-until="Not defined" 
X-OCCI-Attribute: org.fogbowcloud.order.instance-id="1434d5d9-3a81-4cc4-a173-a64a54e41cc6@tu.dresden.manager.naf.lsd.ufcg.edu.br" 
X-OCCI-Attribute: occi.network.gateway="172.16.0.1" 
X-OCCI-Attribute: org.fogbowcloud.order.resource-kind="network" 
X-OCCI-Attribute: org.fogbowcloud.order.user-data="Not defined" 
X-OCCI-Attribute: occi.core.title="Not defined" 
X-OCCI-Attribute: org.fogbowcloud.order.requirements="Not defined" 
X-OCCI-Attribute: org.fogbowcloud.order.instance-count="1" 
X-OCCI-Attribute: occi.network.state="Not defined" 
X-OCCI-Attribute: org.fogbowcloud.order.type="one-time" 
X-OCCI-Attribute: occi.network.address="172.16.0.0/20" 
X-OCCI-Attribute: org.fogbowcloud.order.providing-member="tu.dresden.manager.naf.lsd.ufcg.edu.br" 
X-OCCI-Attribute: occi.networkinterface.interface="Not defined" 
X-OCCI-Attribute: occi.networkinterface.mac="Not defined" 
X-OCCI-Attribute: occi.core.id="Not defined" 
X-OCCI-Attribute: occi.network.allocation="dynamic" 
X-OCCI-Attribute: occi.networkinterface.state="Not defined" 
X-OCCI-Attribute: org.fogbowcloud.credentials.publickey.data="Not defined" 
X-OCCI-Attribute: org.fogbowcloud.order.extra-user-data-content-type="Not defined" 
X-OCCI-Attribute: occi.network.vlan="Not defined" 
X-OCCI-Attribute: org.fogbowcloud.order.valid-from="Not defined" 
X-OCCI-Attribute: org.fogbowcloud.order.storage-size="Not defined"


Create order compute with the new network (1434d5d9-3a81-4cc4-a173-a64a54e41cc6@tu.dresden.manager.naf.lsd.ufcg.edu.br)
./bin/fogbow-cli order --create --n 1 --image fogbow-ubuntu --url http://localhost:8182 --public-key /home/ubuntu/fogbow-instalation/keys/chico.pub --requirements "Glue2RAM >= 1024" --resource-kind compute --auth-token $token --network 1434d5d9-3a81-4cc4-a173-a64a54e41cc6@tu.dresden.manager.naf.lsd.ufcg.edu.br
X-OCCI-Location: http://10.7.41.12:8182/order/7e2da690-ed06-4a2c-80cd-c9eba5221c0d


Get order’s (7e2da690-ed06-4a2c-80cd-c9eba5221c0d) details and check if it is FULFILLED.
./bin/fogbow-cli order --get --auth-token $token --url http://localhost:8182 --id 7e2da690-ed06-4a2c-80cd-c9eba5221c0d
Category: order; scheme="http://schemas.fogbowcloud.org/order#"; class="kind"; title="Order new Instances"; rel="http://schemas.ogf.org/occi/core#resource"; location="http://localhost:8182/order/"; attributes="org.fogbowcloud.order.instance-count org.fogbowcloud.order.type org.fogbowcloud.order.valid-until org.fogbowcloud.order.valid-from org.fogbowcloud.order.state org.fogbowcloud.order.instance-id org.fogbowcloud.credentials.publickey.data org.fogbowcloud.order.user-data org.fogbowcloud.order.extra-user-data org.fogbowcloud.order.extra-user-data-content-type org.fogbowcloud.order.requirements org.fogbowcloud.order.batch-id org.fogbowcloud.order.requesting-member org.fogbowcloud.order.providing-member org.fogbowcloud.order.resource-kind org.fogbowcloud.order.storage-size org.fogbowcloud.order.network-id"
Category: fogbow-ubuntu; scheme="http://schemas.fogbowcloud.org/template/os#"; class="mixin"; title="fogbow-ubuntu image"; rel="http://schemas.ogf.org/occi/infrastructure#os_tpl"; location="http://localhost:8182/fogbow-ubuntu/"
Category: fogbow_public_key; scheme="http://schemas.fogbowcloud/credentials#"; class="mixin"; location="http://localhost:8182/fogbow_public_key/"; attributes="org.fogbowcloud.credentials.publickey.data"
Category: fogbow_userdata; scheme="http://schemas.fogbowcloud.org/order#"; class="mixin"; location="http://localhost:8182/fogbow_userdata/"
X-OCCI-Attribute: occi.network.label="Not defined" 
X-OCCI-Attribute: org.fogbowcloud.order.network-id="1434d5d9-3a81-4cc4-a173-a64a54e41cc6" 
X-OCCI-Attribute: org.fogbowcloud.order.requesting-member="tu.dresden.manager.naf.lsd.ufcg.edu.br" 
X-OCCI-Attribute: org.fogbowcloud.order.state="fulfilled" 
X-OCCI-Attribute: org.fogbowcloud.order.extra-user-data="Not defined" 
X-OCCI-Attribute: org.fogbowcloud.order.batch-id="3b19902a-4b97-4641-b9fa-cc3a2466829e" 
X-OCCI-Attribute: org.fogbowcloud.order.valid-until="Not defined" 
X-OCCI-Attribute: org.fogbowcloud.order.instance-id="c88a6a2a-b9f5-4b75-9f18-d71177789463@tu.dresden.manager.naf.lsd.ufcg.edu.br" 
X-OCCI-Attribute: occi.network.gateway="Not defined" 
X-OCCI-Attribute: org.fogbowcloud.order.resource-kind="compute" 
X-OCCI-Attribute: org.fogbowcloud.order.user-data="TWVzc2FnZS1JRDogPDI5NDE1ODAwOS4wMTQ4MDM1ODQyMzY1MC5KYXZhTWFpbC5qYXZhbWFpbHVzZXJAbG9jYWxob3N0Pg0KTUlNRS1WZXJzaW9uOiAxLjANCkNvbnRlbnQtVHlwZTogbXVsdGlwYXJ0L21peGVkOyANCglib3VuZGFyeT0iLS0tLT1fUGFydF8wXzk2NjY0NjA2OC4xNDgwMzU4NDIzMTY2Ig0KDQotLS0tLS09X1BhcnRfMF85NjY2NDYwNjguMTQ4MDM1ODQyMzE2Ng0KQ29udGVudC1UeXBlOiB0ZXh0L3gtc2hlbGxzY3JpcHQ7IGNoYXJzZXQ9VVRGLTg7IA0KCW5hbWU9MWNsb3VkaW5pdC11c2VyZGF0YS1zY3JpcHQudHh0DQpDb250ZW50LVRyYW5zZmVyLUVuY29kaW5nOiA3Yml0DQpDb250ZW50LURpc3Bvc2l0aW9uOiBhdHRhY2htZW50OyBmaWxlbmFtZT0xY2xvdWRpbml0LXVzZXJkYXRhLXNjcmlwdC50eHQNCg0KIyEvYmluL3NoCmlmY29uZmlnIGV0aDAgbXR1IDE0MjAgfCB0cnVlCklTX09QRU5TU0g9JChzc2ggLXZlciAyPiYxIHwgZ3JlcCBPcGVuU1NIKQpJU19ORVdfT1BFTlNTSD0kKHNzaCAtViAyPiYxIHwgZ3JlcCBPcGVuU1NIKQpJU19EUk9QQkVBUj0kKHNzaCAtdmVyIDI+JjEgfCBncmVwIERyb3BiZWFyKQppZiBbIC1uICIkSVNfT1BFTlNTSCIgXSB8fCBbIC1uICIkSVNfTkVXX09QRU5TU0giIF07IHRoZW4KICBTU0hfT1BUSU9OUz0iLW8gVXNlcktub3duSG9zdHNGaWxlPS9kZXYvbnVsbCAtbyBTdHJpY3RIb3N0S2V5Q2hlY2tpbmc9bm8gLW8gU2VydmVyQWxpdmVJbnRlcnZhbD0zMCIKZWxpZiBbIC1uICIkSVNfRFJPUEJFQVIiIF07IHRoZW4KICBTU0hfT1BUSU9OUz0iLXkgLUsgMzAiCmZpCmNhdCA+IC9iaW4vZm9nYm93LWF1dG9zc2ggPDwgRU9MCiMhL2Jpbi9zaAphdXRvc3NoKCkgewogIHdoaWxlIHRydWU7IGRvCiAgICBSRU1PVEVfUE9SVD0iXCQoY3VybCAtWCBQT1NUIDEwLjcuNDEuMTM6MjIyMy90b2tlbi83ZTJkYTY5MC1lZDA2LTRhMmMtODBjZC1jOWViYTUyMjFjMGQpIgogICAgaWYgWyAteiAiXCRSRU1PVEVfUE9SVCIgXTsgdGhlbgogICAgICBlY2hvICJObyByZW1vdGUgcG9ydCBhdmFpbGFibGUgZm9yIDdlMmRhNjkwLWVkMDYtNGEyYy04MGNkLWM5ZWJhNTIyMWMwZCwgd2lsbCB0cnkgYWdhaW4gaW4gMzAgc2Vjb25kcyIKICAgICAgc2xlZXAgMzAKICAgICAgY29udGludWUKICAgIGZpCiAgICBlY2hvICJTdGFydGluZyB0dW5uZWwgaW4gcG9ydCBcJFJFTU9URV9QT1JUIgogICAgCiAgICBPTERfSUZTPVwkSUZTICAgICAgICAgICAgICAjIHNhdmUgaW50ZXJuYWwgZmllbGQgc2VwYXJhdG9yCiAgICBJRlM9IjoiICAgICAgICAgICAgICAgICAgICMgc2V0IGl0IHRvICc6JwogICAgc2V0IC0tIFwkUkVNT1RFX1BPUlQgICAgICAgIyBtYWtlIHRoZSByZXN1bHQgcG9zaXRpb25hbCBwYXJhbWV0ZXJzCiAgICBJRlM9XCRPTERfSUZTICAgICAgICAgICAgICAjIHJlc3RvcmUgSUZTCiAgICBQT1JUPVwkMQogICAgU1NIX1NFUlZFUl9QT1JUPVwkMgogICAgCiAgICBlY2hvICJDb21tYW5kOiBzc2ggJFNTSF9PUFRJT05TIC1OIC1SIDAuMC4wLjA6XCRQT1JUOmxvY2FsaG9zdDoyMiA3ZTJkYTY5MC1lZDA2LTRhMmMtODBjZC1jOWViYTUyMjFjMGRAMTAuNy40MS4xMyAtcCBcJFNTSF9TRVJWRVJfUE9SVCIKICAgIHNzaCAkU1NIX09QVElPTlMgLU4gLVIgMC4wLjAuMDpcJFBPUlQ6bG9jYWxob3N0OjIyIDdlMmRhNjkwLWVkMDYtNGEyYy04MGNkLWM5ZWJhNTIyMWMwZEAxMC43LjQxLjEzIC1wIFwkU1NIX1NFUlZFUl9QT1JUCiAgICBzbGVlcCA1CiAgZG9uZQp9CmF1dG9zc2ggJgpFT0wKY2htb2QgK3ggL2Jpbi9mb2dib3ctYXV0b3NzaApzZXRzaWQgL2Jpbi9mb2dib3ctYXV0b3NzaApjYXQgPiAvYmluL2NyZWF0ZS1mb2dib3ctdHVubmVsIDw8IEVPTAojIS9iaW4vc2gKVE9LRU5fVkFSPVwkMQpMT0NBTF9QT1JUPVwkMgplY2hvICJSZXF1ZXN0aW5nIHBvcnQgY3VybCAtWCBQT1NUIDEwLjcuNDEuMTM6MjIyMy90b2tlbi83ZTJkYTY5MC1lZDA2LTRhMmMtODBjZC1jOWViYTUyMjFjMGQtXCRUT0tFTl9WQVIiClJFTU9URV9QT1JUPVwkKGN1cmwgLVggUE9TVCAxMC43LjQxLjEzOjIyMjMvdG9rZW4vN2UyZGE2OTAtZWQwNi00YTJjLTgwY2QtYzllYmE1MjIxYzBkLVwkVE9LRU5fVkFSKQpPTERfSUZTPVwkSUZTICAgICAgICAgICAgICAjIHNhdmUgaW50ZXJuYWwgZmllbGQgc2VwYXJhdG9yCklGUz0iOiIgICAgICAgICAgICAgICAgICAgIyBzZXQgaXQgdG8gJzonCnNldCAtLSBcJFJFTU9URV9QT1JUICAgICAgICMgbWFrZSB0aGUgcmVzdWx0IHBvc2l0aW9uYWwgcGFyYW1ldGVycwpJRlM9XCRPTERfSUZTICAgICAgICAgICAgICAjIHJlc3RvcmUgSUZTClBPUlQ9XCQxClNTSF9TRVJWRVJfUE9SVD1cJDIKZWNobyAiUmVjaXZlcyBQT1JUOlwkUE9SVCBhbmQgU1NIIFNFUlZFUiBQT1JUOiBcJFNTSF9TRVJWRVJfUE9SVCIKZWNobyAiQ29tbWFuZDogc3NoICRTU0hfT1BUSU9OUyAtZiAtTiAtUiAwLjAuMC4wOlwkUE9SVDpsb2NhbGhvc3Q6XCRMT0NBTF9QT1JUIDdlMmRhNjkwLWVkMDYtNGEyYy04MGNkLWM5ZWJhNTIyMWMwZC1cJFRPS0VOX1ZBUkAxMC43LjQxLjEzIC1wIFwkU1NIX1NFUlZFUl9QT1JUIgoKc3NoICRTU0hfT1BUSU9OUyAtZiAtTiAtUiAwLjAuMC4wOlwkUE9SVDpsb2NhbGhvc3Q6XCRMT0NBTF9QT1JUIDdlMmRhNjkwLWVkMDYtNGEyYy04MGNkLWM5ZWJhNTIyMWMwZC1cJFRPS0VOX1ZBUkAxMC43LjQxLjEzIC1wIFwkU1NIX1NFUlZFUl9QT1JUCkVPTApjaG1vZCAreCAvYmluL2NyZWF0ZS1mb2dib3ctdHVubmVsCg0KLS0tLS0tPV9QYXJ0XzBfOTY2NjQ2MDY4LjE0ODAzNTg0MjMxNjYNCkNvbnRlbnQtVHlwZTogdGV4dC9jbG91ZC1jb25maWc7IGNoYXJzZXQ9VVRGLTg7IA0KCW5hbWU9MmNsb3VkaW5pdC1jbG91ZC1jb25maWcudHh0DQpDb250ZW50LVRyYW5zZmVyLUVuY29kaW5nOiA3Yml0DQpDb250ZW50LURpc3Bvc2l0aW9uOiBhdHRhY2htZW50OyBmaWxlbmFtZT0yY2xvdWRpbml0LWNsb3VkLWNvbmZpZy50eHQNCg0KI2Nsb3VkLWNvbmZpZwp1c2VyczoKICAtIG5hbWU6IGZvZ2JvdwogICAgY2hwYXNzd2Q6IHtleHBpcmU6IEZhbHNlfQogICAgc3NoX3B3YXV0aDogVHJ1ZQogICAgc2hlbGw6IC9iaW4vYmFzaAogICAgc3VkbzogQUxMPShBTEwpIE5PUEFTU1dEOkFMTAogICAgc3NoX2F1dGhvcml6ZWRfa2V5czoKICAgICAgLSBzc2gtcnNhIEFBQUFCM056YUMxeWMyRUFBQUFEQVFBQkFBQUJBUURTWEFlZGswNW9pSGVJNzZURlU4c09aaS9LeDFSMUIxUXh5Sm12M2lVMVpPTEhScTVYNmc2U3phdnE0a2s0VE5lRFNyZHIyV1VqbnFhVFdIYjIrbnJHQWsxT1hyVktReWtZVU85djlTaDRTSnZTdGcrWTBMTVgyV0Y0aG5QK1UvVzlURTh5aWU1Mm04T1JKclBWN0tJei9yVHp6NEFDdm14RHdPa1hKRmE1c25YS2toakFGNFc5aDBlT1BxMmtNV3l5N1phZXFpNEdiRm1UcVFTaEhtamg5YTMxUWpXOHJTRnZYNGN2YjdzTXZvVjlnN2x3SmdBRDNVaThTZXk3QjIvTjhDbURQQ2hyRWdEaVB4bmE2SElBdWh0ZnZIS1JDczBzN3JvTTdmYjZ1dXRLWmxINmE4WGcrSXZIV0VyakwwVWl3S0VQblRuamM4SjQ1ajYwcFUwbCBjaGljb2dAdGFpb2JhDQotLS0tLS09X1BhcnRfMF85NjY2NDYwNjguMTQ4MDM1ODQyMzE2Ni0tDQo=" 
X-OCCI-Attribute: occi.core.title="Not defined" 
X-OCCI-Attribute: org.fogbowcloud.order.requirements="Glue2RAM >= 1024" 
X-OCCI-Attribute: org.fogbowcloud.order.instance-count="1" 
X-OCCI-Attribute: occi.network.state="Not defined" 
X-OCCI-Attribute: org.fogbowcloud.order.type="one-time" 
X-OCCI-Attribute: occi.network.address="Not defined" 
X-OCCI-Attribute: org.fogbowcloud.order.providing-member="tu.dresden.manager.naf.lsd.ufcg.edu.br" 
X-OCCI-Attribute: occi.networkinterface.interface="Not defined" 
X-OCCI-Attribute: occi.networkinterface.mac="Not defined" 
X-OCCI-Attribute: occi.core.id="Not defined" 
X-OCCI-Attribute: occi.network.allocation="Not defined" 
X-OCCI-Attribute: occi.networkinterface.state="Not defined" 
X-OCCI-Attribute: org.fogbowcloud.credentials.publickey.data="ssh-rsa AAAAB3NzaC1yc2EAAAADAQABAAABAQDSXAedk05oiHeI76TFU8sOZi/Kx1R1B1QxyJmv3iU1ZOLHRq5X6g6Szavq4kk4TNeDSrdr2WUjnqaTWHb2+nrGAk1OXrVKQykYUO9v9Sh4SJvStg+Y0LMX2WF4hnP+U/W9TE8yie52m8ORJrPV7KIz/rTzz4ACvmxDwOkXJFa5snXKkhjAF4W9h0eOPq2kMWyy7Zaeqi4GbFmTqQShHmjh9a31QjW8rSFvX4cvb7sMvoV9g7lwJgAD3Ui8Sey7B2/N8CmDPChrEgDiPxna6HIAuhtfvHKRCs0s7roM7fb6uutKZlH6a8Xg+IvHWErjL0UiwKEPnTnjc8J45j60pU0l chicog@taioba" 
X-OCCI-Attribute: org.fogbowcloud.order.extra-user-data-content-type="Not defined" 
X-OCCI-Attribute: occi.network.vlan="Not defined" 
X-OCCI-Attribute: org.fogbowcloud.order.valid-from="Not defined" 
X-OCCI-Attribute: org.fogbowcloud.order.storage-size="Not defined"


Delete compute instance(c88a6a2a-b9f5-4b75-9f18-d71177789463@tu.dresden.manager.naf.lsd.ufcg.edu.br)
./bin/fogbow-cli instance --delete --auth-token $token --url http://localhost:8182 --id c88a6a2a-b9f5-4b75-9f18-d71177789463@tu.dresden.manager.naf.lsd.ufcg.edu.br
Ok
Delete network instance(1434d5d9-3a81-4cc4-a173-a64a54e41cc6@tu.dresden.manager.naf.lsd.ufcg.edu.br)
./bin/fogbow-cli network --delete --auth-token $token --url http://localhost:8182 --id 1434d5d9-3a81-4cc4-a173-a64a54e41cc6@tu.dresden.manager.naf.lsd.ufcg.edu.br
Ok


Delete order(7e2da690-ed06-4a2c-80cd-c9eba5221c0d)
./bin/fogbow-cli order --delete --auth-token $token --url http://localhost:8182 --id 7e2da690-ed06-4a2c-80cd-c9eba5221c0d
Ok
