# Prefix List

### Todas as redes 
```
ip prefix-list <Nome> permit 0.0.0.0/0 le 32
```

### Nega a rfc1918
```
ip prefix-list rfc1918 deny 0.0.0.0/8 le 32
ip prefix-list rfc1918 deny 10.0.0.0/8 le 32
ip prefix-list rfc1918 deny 127.0.0.0/8 le 32
ip prefix-list rfc1918 deny 169.254.0.0/16 le 32
ip prefix-list rfc1918 deny 172.16.0.0/12 le 32
ip prefix-list rfc1918 deny 192.0.2.0.0/24 le 32
ip prefix-list rfc1918 deny 192.168.0.0/16 le 32
ip prefix-list rfc1918 deny 224.0.0.0/3 le 32
ip prefix-list rfc1918 permit 0.0.0.0/0 le 32
``` 
