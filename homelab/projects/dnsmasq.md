---
layout: default
title: dnsmasq
nav_order: 1
parent: Projects
---

> [!NOTE]
> **Page in Development**  
> This page is still under construction.
> Please check in again soon for content.

### DNSMASQ

#### Overview


/etc/dnsmasq.conf - points to dnsmasq.d/ to use additional configuration files.

dnsmasq.d/
-- arecords.conf
-- dhcp.conf
-- dns.conf
-- leases.conf
-- logging.conf
-- main.conf
-- ptr-records.conf


#### dnsmasq.conf
```

```


#### main.conf
```
#Main Configuration
no-resolv #DNSMASQ does not read resolv.conf for ___.
no-hosts #DNSMASQ does not read /etc/hosts for device

listen-address=127.0.0.1,[Address of DNS Server]
port=53 #Typically 53 for DNS traffic
#Backup DNS Servers for when DNSMASQ fails to resolve a query.
server=1.1.1.1 #Cloudflare
server=8.8.8.8 #Google
```

I've sorted the main configuration options that cover dnsmasq overall in the above file. 


#### dns.conf
```

```

#### arecords.conf
```
#a records
#The format for an A record in DNSMASQ is the following:
#address=/[sub.yourdomain.suffix (usually lan or internal)]/[IPv4 Address.]
#For example, these are some of my own records:
address=/raspberrypi.customdomain.lan/192.168.1.x
address=/raspberrypi2.customdomain.lan/192.168.1.y
address=/netconfig.customdomain.lan/192.168.1.z
address=/printer.customdomain.lan/192.168.1.a
address=/computer.customdomain.lan/192.168.1.b
```

An A record basically allows you to map an IP address to an assigned domain. In my case, I used it to link my device(s) to a paraticular domain, I have a subdomain denoting each device. I can then use the domain to access something on a particular device instead of having to frequently check what it's IP address is.

#### ptr-records.conf
```
#PTR records, AKA pointer records.

```

Pointer records are basically the opposite of an A record. With A records, you can use a domain and it finds the matching IP address. With pointer records, it takes an IP address and finds the matching domain. While we typically avoid typing in IP addresses manually, the pointer records can be helpful in situations where the device is referenced by the IP Address instead of the domain. By adding a pointer record, the device can be labelled by it's domain increasing readability of logs when troubleshooting. [Cloudflare](https://www.cloudflare.com/learning/dns/dns-records/dns-ptr-record/) has a really good  article on the topic that I'd recommend reading.

The record flow is like the example below:

**A record:**   device.domain.suffix -> 192.168.1.99
**PTR record:** 192.168.1.99 -> device.domain.suffix

#### dhcp.conf
```
#DHCP
dhcp-range=192.168.1.32, 192.168.1.63,48h #change to 2 - 31 and set

dhcp-option=3,192.168.1.254 #This sets the default gateway
dhcp-ignore=tag:!known

#wpad is a config protocol to determine the proxy and config a device uses, in which can be manipulated if not careful.
#Since I am not using a web proxy, I have the below options enabled to reduce the risk.
dhcp-name-match=set:wpad=ignore,wpad
dhcp-ignore-names=tag:wpad-ignore
```

DNSMASQ has both dns and dhcp functionalities.

DHCP allows you to automatically configure the IP addresses, DNS servers, and default gateways for different devices.
It basically takes a range of addresses and when a device needs an IP address, will hand them out. The issue is when a device disconnects but is still using that IP Address. This is a frequent occurrence for portable or guest devices that frequently go off network. To solve this issue, the IP address is instead borrowed or "leased". The "leased" address will eventually run out of time and so the dhcp server will let the device keep the address and renew it or will give them a new one and put it back in the list of addresses it can hand out. What happens when we want a device to keep it's IP Address though? Perhaps for the purpose of tying it to a domain. To do this, we can set a static lease or reservation, in which the device keeps it's assigned address. This means that the domain associated with that particular address doesn't accidentally point to a different device.

A record: device.yourdomain.tld -> 192.168.1.99
PTR record: 192.168.1.99 -> device.yourdomain.tld
DHCP Reservation: Device Identifier (MAC Address usually) -> 192.168.1.99

#### leases.conf
```
#DHCP Static Leases
dhcp-host=[mac address],192.168.1.33,pi #33
dhcp-host=[mac address],192.168.1.34,printer #34
dhcp-host=[mac address],192.168.1.35,laptop #35
dhcp-host=[mac address],192.168.1.36,steamdeck #36
dhcp-host=[mac address],192.168.1.37,iphone #37
```

These reservations are in the following format:
`dhcp-host=[MAC address],[IPv4 address],[device name]`

Since IP Addresses change all the time, we need another alternative to identify different devices on the network. A MAC address is basically a unique address assigned to the network interface card a device has.

#### logging.conf
```
#logging options
#Uncomment the below lines for when completing testing/debugging of either service
log-queries #log dns queries/requests
log-dhcp #log dhcp related requests (device requests IP address, dns server, etc.)

log-facility=/var/log/dnsmasq.log #location of log file to output logged traffic to.
```

I set these options in a separate file so I'm not opening up another file and accidentally changing one of the options I don't want to change. I also wrote a script so that I can easily toggle it on or toggle it off in the event I need to do a bit of troubleshooting.

>Please update the below link:


Please see my toggle script for more info: [toggle_dnsmasq_logging](https://github.com/Smyles1105/scripts/blob/main/toggle_dnsmasq_logging.sh)
