---
title: "Building My First IPv6 Cybersecurity Homelab"
date: 2025-07-14
layout: single
author_profile: true
read_time: true
categories: 
  - homelab
  - networking
  - ipv6
  - cybersecurity
tags:
  - virtualbox
  - metasploitable
  - kali
  - ubuntu
  - windows
  - linux
---

## Building My First IPv6 Cybersecurity Homelab

Over the past week, I set up a fully connected virtual lab running entirely over IPv6. My goal was to create a realistic environment where I could experiment with offensive and defensive techniques, break things, fix them, and learn by doing. This post documents the full process, from configuring the network to validating communication across multiple operating systems.

---

### Why IPv6?

Most beginner homelabs rely on IPv4 with NAT or bridged adapters. I wanted something closer to how enterprise environments are evolving: routable addressing, no NAT, and modern protocol stacks. IPv6 forces me to work with real-world addressing, subnetting, and connectivity issues — and it's something many engineers still avoid.

---

### The Lab Architecture

All machines run in **VirtualBox** using an **Internal Network** (no internet). Here's the breakdown:

| OS               | Purpose                        | IPv6 Address                |
|------------------|--------------------------------|-----------------------------|
| Ubuntu Server    | Core system, potential gateway | `fd12:3456:789a:1::1/64`    |
| Ubuntu Desktop   | Client testing, GUI tools      | `fd12:3456:789a:1::2/64`    |
| Kali Linux       | Offensive tools & enumeration  | `fd12:3456:789a:1::3/64`    |
| Windows 10       | Real-world target system       | `fd12:3456:789a:1::4/64`    |
| Metasploitable 2 | Vulnerable machine             | `fd12:3456:789a:1::5/64`    |

All VMs are on the same internal VirtualBox network (`intnet`) and assigned static IPv6 addresses. No DHCPv6 or router advertisements were needed.

---

### Key Configuration Lessons

I configured each VM manually. These were the most important lessons and fixes I ran into:

- **Netplan syntax errors**: YAML is strict. Use spaces, not tabs. Bad indentation = broken network.
- **Permissions**: `chmod 600` for netplan config files removed warnings about unsafe permissions.
- **Missing interfaces**: Metasploitable needed `eth0` added manually in `/etc/network/interfaces`.
- **Ping failures**: Windows 10 blocked ICMPv6 by default. I had to enable incoming echo requests in the firewall settings.

Each system had slightly different syntax or quirks, so I documented every config line for future reference.

---

### Connectivity Tests

To verify everything was working:

- Used `ping6` from each VM to all others
- From Kali, ran: `nmap -6 -sV fd12:3456:789a:1::/64` to discover active hosts and services
- Confirmed consistent packet return and zero drops after firewall fixes

✅ All systems can talk to each other via IPv6 without NAT, using their static addresses.

---

### Screenshots 📸 (optional but recommended)

I suggest adding 2–3 high-quality screenshots to enhance the post:

1. **VirtualBox Network Configuration**  
   Show all VMs connected to `intnet` under “Network > Adapter 1”.

2. **IPv6 ping from Kali to Metasploitable**  
   Highlight command + successful return.

3. **Netplan YAML in Ubuntu Server**  
   Display `/etc/netplan/01-intnet.yaml` with your custom IPv6 config.

To embed an image (place inside `/assets/images/`):

```markdown
![Kali pinging Metasploitable over IPv6](/assets/images/kali-ping-ipv6.png)
