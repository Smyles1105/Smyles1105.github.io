---
layout: default
title: Changelog
parent: Homelab
nav_order: 1
---

## Change Log

### 2025-05-27 Thinkcentre m920q setup

- Booted into preinstalled Windows 11 Pro to confirm hardware specs (i5-8500t, 32GB RAM, 512GB SSD)
- Created Proxmox installation media and installed Proxmox VE
- Deployed existing VMs: Windows 10 Enterprise LTSC, Windows Server 2019 LTSC, and Ubuntu MATE Desktop
- Encountered storage driver issues with Windows VMs; resolved using [Proxmox best practices](https://pve.proxmox.com/wiki/Windows_2019_guest_best_practices) and VirtIO drivers.

**Laptop Windows reinstallation prep - deferred until thinkcentre became proxmox host**
- Required Proxmox on Thinkcentre to wipe Proxmox on laptop.
- Identified USB drive failure when preparing Windows 11 media for laptop reinstall (needed for OnVue Exams (A+))
- Ordered replacement USBs for OS setup and file transfers.
- Reinstallation planned after drives arrive.


### 2025-05-21 VPN Testing (WireGuard via Pi)

- configured PiVPN (WireGuard) on Raspberry Pi after reimaging with static IP and UDP 51825 - also tested ports 443 and 51820 
- confirmed public IP on router, firewall disabled, DMZ enabled to Pi
- tested from iPhone on LTE — no traffic seen in tcpdump on above ports
- concluded VPN not possible due to ISP/carrier restrictions or router bug
- shifting Pi toward other useful lightweight services (e.g. DNS, metrics, logging)
- DNSMasq was cleaned up, config files in customdnsmasq.d are now in the default dnsmasq.d folder.
- Raspberry Pi also now is only using eth0 Ethernet interface - wlan0 disabled, will re-enable if needed.

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
- wanting to add ethernet to the raspberry pi for faster and more reliable network connection.
