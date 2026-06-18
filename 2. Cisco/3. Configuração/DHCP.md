# DHCP

### DHCP IP Estaticos
 
http://www.cisco.com/c/en/us/td/docs/ios-xml/ios/ipaddr_dhcp/configuration/15-sy/dhcp-15-sy-book/config-dhcp-server.html
The following example shows how to create a manual binding for a client named example2.abc.com that does not send a client identifier in the DHCP packet. The MAC address of the client is 02c7.f800.0422 and the IP address of the client is 172.16.2.253.

```
!
ip dhcp pool TESTE_EBT
   host 192.168.24.25 255.255.255.0
   client-identifier 01e8.9a8f.86dc.9b
   default-router 192.168.24.10
   dns-server 192.168.1.12
 
ou
 
ip dhcp pool pool2 
 host 172.16.2.253 
 hardware-address 02c7.f800.0422 ethernet
 client-name example1
```
