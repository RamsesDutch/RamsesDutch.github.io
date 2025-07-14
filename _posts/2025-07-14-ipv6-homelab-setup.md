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

### Ubuntu Server Netplan Configuration

Here is the exact configuration I used on the Ubuntu Server to assign its static IPv6 address:

![Ubuntu Server netplan configuration](/assets/images/ubuntu-server-netplan.png)

---

Connectivity Tests
After configuring all the VMs, I tested connectivity between them using IPv6 ping commands. Here are screenshots showing:

Kali Linux successfully pinging Metasploitable 2
![Kali ping to metasploitable](/assets/images/ping-kali-to-metasploitable.PNG)

Ubuntu Desktop successfully pinging Windows 10

These confirmed that the IPv6 network is fully functional, and all machines can communicate without NAT.

What’s Next?
With this IPv6 homelab ready, I plan to move on to practical projects like port scanning, vulnerability assessments, and network monitoring — all within a realistic IPv6 environment.

Final Thoughts
Setting up this homelab was a great learning experience. Getting IPv6 right requires attention to detail, but it’s invaluable for anyone serious about networking and cybersecurity. The ability to build, break, and fix systems in a controlled lab environment accelerates understanding far beyond theory.
