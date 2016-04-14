# DNS and DCHP servers with failover for home network

Useful Sources:
- http://blogging.dragon.org.uk/dns-with-bind9-and-dhcp-on-ubuntu-14-04/
- http://frankhinek.com/how-to-setup-a-dns-server-for-a-home-lab-on-ubuntu-14-04/  <-- has useful stuff about forwarders and additional lockdown
- https://blog.bigdinosaur.org/running-bind9-and-isc-dhcp/
- Failover
  - http://serverfault.com/a/667478
  - https://kb.isc.org/article/AA-00502/0/A-Basic-Guide-to-Configuring-DHCP-Failover.html
https://www.madboa.com/geek/dhcp-failover/
- http://docstore.mik.ua/orelly/networking_2ndEd/dns/ch11_02.htm
- http://blog.philippklaus.de/2014/08/deploy-your-own-bind9-based-ddns-server/
- https://www.digitalocean.com/community/tutorials/how-to-configure-bind-as-an-authoritative-only-dns-server-on-ubuntu-14-04
- https://help.ubuntu.com/community/StaticDnsWithDhcp <-- use for setting to `prepend` Google nameserver for plex (DNS rebind error otherwise)
- http://www.zytrax.com/books/dns/ch8/cname.html

## Variables

|Local domain|xmansion.local|
|-|-|
|server-1 (primary)|charles|
|server-2 (secondary)|eric|
|network|192.168.0/24|

## Remember
**Apparmour**: Evil errors when starting up isc-dhcp-server

```
sudo nano /etc/apparmor.d/usr.sbin.dhcpd
...
# Comment out below line and replace
#  /etc/dhcp/ddns-keys/** r,
   /etc/bind/rndc.key  r,
...
sudo nano /etc/apparmor.d/usr.sbin.named
...
# Add a line for zones to be rw (allows for dynamic updates)
/etc/bind/zones/**  rw,
```

`sudo service apparmor restart`

http://stackoverflow.com/q/31631891

## Key files

### Bind DNS
```
/etc/bind/rndc.key  # only on primary, copy to secondary
/etc/bind/named.conf.options
/etc/bind/named.conf.local
/etc/bind/zones/*  # only on primary
```
### DHCP
```
/etc/dhcp/dhcpd.conf
```
