# Rate Limit

```
interface <Interface>
qos car inbound any cir 256 cbs 16000 ebs 0 green pass red discard yellow pass
qos car outbound any cir 256 cbs 16000 ebs 0 green pass red discard yellow pass
```
