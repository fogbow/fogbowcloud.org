Title: Deployment Test
url: deployment-test
save_as: deplyment-test.html

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

10. Create attachment with compute(f32eb79d-5681-488a-a6e3-fd0fe8426b96@tu.dresden.manager.naf.lsd.ufcg.edu.br) and storage(ef01b2b6-ab77-4705-92a2-83642d88e2ef@tu.dresden.manager.naf.lsd.ufcg.edu.br).
```shell
./bin/fogbow-cli attachment --create --auth-token $token --url http://localhost:8182 --computeId f32eb79d-5681-488a-a6e3-fd0fe8426b96@tu.dresden.manager.naf.lsd.ufcg.edu.br --storageId ef01b2b6-ab77-4705-92a2-83642d88e2ef@tu.dresden.manager.naf.lsd.ufcg.edu.br

http://10.7.41.12:8182/storage/link//ef01b2b6-ab77-4705-92a2-83642d88e2ef@tu.dresden.manager.naf.lsd.ufcg.edu.br
```


11. Get attachment’s(ef01b2b6-ab77-4705-92a2-83642d88e2ef@tu.dresden.manager.naf.lsd.ufcg.edu.br) details
```shell
./bin/fogbow-cli attachment --get --auth-token $token --id ef01b2b6-ab77-4705-92a2-83642d88e2ef@tu.dresden.manager.naf.lsd.ufcg.edu.br --url http://localhost:8182

storagelink; scheme="http://schemas.ogf.org/occi/infrastructure#"; class="kind"; title="A link to a storage resource"; rel="http://schemas.ogf.org/occi/core#link"; location="http://localhost:8182/storage/link/"; attributes="occi.storagelink.state{immutable} occi.storagelink.mountpoint occi.storagelink.deviceid"
X-OCCI-Attribute: occi.storagelink.provadingMemberId="tu.dresden.manager.naf.lsd.ufcg.edu.br"
X-OCCI-Attribute: occi.storagelink.deviceid="null"
X-OCCI-Attribute: occi.core.target="ef01b2b6-ab77-4705-92a2-83642d88e2ef"
X-OCCI-Attribute: occi.core.source="f32eb79d-5681-488a-a6e3-fd0fe8426b96"
```

12. Delete attachment(ef01b2b6-ab77-4705-92a2-83642d88e2ef@tu.dresden.manager.naf.lsd.ufcg.edu.br)
```shell
./bin/fogbow-cli attachment --delete --auth-token $token --url http://localhost:8182 --id ef01b2b6-ab77-4705-92a2-83642d88e2ef@tu.dresden.manager.naf.lsd.ufcg.edu.br

Ok
```

13. Delete instance (f32eb79d-5681-488a-a6e3-fd0fe8426b96@tu.dresden.manager.naf.lsd.ufcg.edu.br).
```shell
./bin/fogbow-cli instance --delete --auth-token $token --url http://localhost:8182 --id f32eb79d-5681-488a-a6e3-fd0fe8426b96@tu.dresden.manager.naf.lsd.ufcg.edu.br
Ok
```

14. Delete order (65a746b4-4b84-455a-b36a-bf64f030ab56).
```shell
./bin/fogbow-cli order --delete --auth-token $token --url http://localhost:8182 --id 65a746b4-4b84-455a-b36a-bf64f030ab56

Ok
```

15. Delete storage(ef01b2b6-ab77-4705-92a2-83642d88e2ef@tu.dresden.manager.naf.lsd.ufcg.edu.br)
```shell
./bin/fogbow-cli storage --delete --auth-token $token --url http://localhost:8182 --id ef01b2b6-ab77-4705-92a2-83642d88e2ef@tu.dresden.manager.naf.lsd.ufcg.edu.br

Ok
```

16. Create order network
```shell
./bin/fogbow-cli order --create --n 1 --resource-kind network --cidr 172.16.0.0/12 --gateway 172.16.0.0 --allocation dynamic --url http://localhost:8182 --auth-token $token

X-OCCI-Location: http://10.7.41.12:8182/order/0ee0578b-38ed-4d77-98c7-cc4c51f57c2d
```

17. Get order’s (0ee0578b-38ed-4d77-98c7-cc4c51f57c2d) details and check if it is FULFILLED.
```shell
./bin/fogbow-cli order --get --auth-token $token --url http://localhost:8182 --id 0ee0578b-38ed-4d77-98c7-cc4c51f57c2d
[...]
X-OCCI-Attribute: org.fogbowcloud.order.state="fulfilled" 
[...]
X-OCCI-Attribute: org.fogbowcloud.order.instance-id="1434d5d9-3a81-4cc4-a173-a64a54e41cc6@tu.dresden.manager.naf.lsd.ufcg.edu.br" 
[...]
```

18. Create order compute with the new network (1434d5d9-3a81-4cc4-a173-a64a54e41cc6@tu.dresden.manager.naf.lsd.ufcg.edu.br)
```shell
./bin/fogbow-cli order --create --n 1 --image fogbow-ubuntu --url http://localhost:8182 --public-key /home/ubuntu/fogbow-instalation/keys/chico.pub --requirements "Glue2RAM >= 1024" --resource-kind compute --auth-token $token --network 1434d5d9-3a81-4cc4-a173-a64a54e41cc6@tu.dresden.manager.naf.lsd.ufcg.edu.br

X-OCCI-Location: http://10.7.41.12:8182/order/7e2da690-ed06-4a2c-80cd-c9eba5221c0d
```

20. Get order’s (7e2da690-ed06-4a2c-80cd-c9eba5221c0d) details and check if it is FULFILLED.
```shell
./bin/fogbow-cli order --get --auth-token $token --url http://localhost:8182 --id 7e2da690-ed06-4a2c-80cd-c9eba5221c0d

[...]
X-OCCI-Attribute: org.fogbowcloud.order.state="fulfilled" 
[...]
X-OCCI-Attribute: org.fogbowcloud.order.instance-id="c88a6a2a-b9f5-4b75-9f18-d71177789463@tu.dresden.manager.naf.lsd.ufcg.edu.br" 
[...]
```

21. Delete compute instance(c88a6a2a-b9f5-4b75-9f18-d71177789463@tu.dresden.manager.naf.lsd.ufcg.edu.br)
```shell
./bin/fogbow-cli instance --delete --auth-token $token --url http://localhost:8182 --id c88a6a2a-b9f5-4b75-9f18-d71177789463@tu.dresden.manager.naf.lsd.ufcg.edu.br

Ok
```

22. Delete network instance(1434d5d9-3a81-4cc4-a173-a64a54e41cc6@tu.dresden.manager.naf.lsd.ufcg.edu.br)
```shell
./bin/fogbow-cli network --delete --auth-token $token --url http://localhost:8182 --id 1434d5d9-3a81-4cc4-a173-a64a54e41cc6@tu.dresden.manager.naf.lsd.ufcg.edu.br

Ok
```


23. Delete order(7e2da690-ed06-4a2c-80cd-c9eba5221c0d)
```shell
./bin/fogbow-cli order --delete --auth-token $token --url http://localhost:8182 --id 7e2da690-ed06-4a2c-80cd-c9eba5221c0d

Ok
```
