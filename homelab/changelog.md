---
layout: default
title: Changelog
parent: Homelab Overview
nav_order: 1
---

## Change Log

### 2025-05-21 VPN Testing (WireGuard via Pi)

- configured PiVPN (WireGuard) on Raspberry Pi after reimagine with static IP and UDP 51825 - also tested ports 443 and 51820 
- confirmed public IP on router, firewall disabled, DMZ enabled to Pi
- tested from iPhone on LTE — no traffic seen in tcpdump on port 51825
- tested Tailscale as fallback — didn’t work either
- concluded VPN not possible due to ISP/carrier restrictions or router bug
- shifting Pi toward other useful lightweight services (e.g. DNS, metrics, logging)
- DNSMasq was cleaned up, config files in customdnsmasq.d are now in the default dnsmasq.d folder.
- Raspberry Pi also now is only using eth0 Ethernet interface - wlan0 disabled, if a use for it arises I will enable it

### 2025-05-03 Config backup/cleanup
- Backed up /etc/customdnsmasq.d/ and /home/smyles11/scripts to google drive in a new homelab folder
- ensured logging was disabled and restarted dnsmasq
- removed old pihole config - dnsmasq.d/ is now empty but it needs to stay there because dnsmasq will check that folder for configs even with a separate config folder
- tested using `dnsmasq --test` and `systemctl status`

### 2025-05-02 DNS Speed improvements
- set cache size as 1000 in dns.conf due to high ping
- set neg-ttl as 600 seconds (10 minutes)
- added 8.8.8.8 as one of the backup servers (1.1.1.1, 8.8.8.8)
- ensured main config was not using /etc/resolv.conf or /etc/hosts
- wanting to migrate raspberry pi to under the stairs for ethernet to ensure faster dns queries.
