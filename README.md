# Cybersecurity Lab Setup - My First Homelab

**Author:** Alvin Supeeti Karkures - Cybersecurity Student, Nairobi
**Host OS:** Windows 10 Home on Fujitsu Lifebook U938, i5 7th Gen, 8GB RAM, 256GB SSD
**Virtualization:** Oracle VirtualBox 7.x - NAT Network
**Lab OS:** Kali Linux 2026.x

## Lab Goal
Set up an isolated cybersecurity lab with static IP 10.0.0.2/24 as instructed, with internet connectivity for tools.

## Lab Architecture
- VirtualBox NAT Network: 10.0.0.0/24 (Changed from default 10.0.2.0/24)
- Gateway: 10.0.0.1
- Kali IP: 10.0.0.2/24
- DNS: 8.8.8.8

## Setup Steps I Did
1. Created NAT Network in VirtualBox > Tools > Network Manager
2. Initially it showed IPv4 Prefix 10.0.2.0/24 - noticed mismatch
3. Changed it to 10.0.0.0/24 and Applied
4. Configured Kali static IP:
   - sudo nmcli con del ethernet
   - sudo nmcli con add type ethernet con-name ethernet ifname eth0 ipv4.addresses 10.0.0.2/24 ipv4.gateway 10.0.0.1 ipv4.dns 8.8.8.8 ipv4.method manual
   - sudo nmcli con mod ethernet ipv4.dad-timeout 0
   - sudo nmcli con up ethernet


## Problems I Faced & How I Fixed Them (REAL LEARNING)

**Problem 1: Destination Host Unreachable**
- When I set 10.0.0.2 while VirtualBox was still on 10.0.2.0/24, ping to 10.0.0.1 failed.
- Reason: Different networks - like trying to find a house on a different street.
- Fix: Changed VirtualBox IPv4 Prefix to 10.0.0.0/24

**Problem 2: 10.0.0.2 Reserved Conflict**
- 10.0.0.2 is often the NAT gateway/DHCP itself in VirtualBox.
- Needed `ipv4.dad-timeout 0` to bypass Duplicate Address Detection.

**Problem 3: Copy/Paste not working**
- Ctrl+C/V doesn't work in Kali terminal. Learned to use Ctrl+Shift+C / Ctrl+Shift+V
- Enabled Devices > Shared Clipboard > Bidirectional and Drag & Drop

**Problem 4: GUI says Disconnected but internet works**
- Top right icon showed "Disconnected" even though `ping 8.8.8.8` worked.
- Learned to trust terminal output (`ip a` and `ping`) over GUI.

## Verification (My Screenshots)
- Screenshots folder
1. `ip_a_10.0.0.2.png` - shows inet 10.0.0.2/24
2. `ping_gateway.png` - ping 10.0.0.1 replies
3. `ping_internet.png` - ping 8.8.8.8 and google.com
4. `virtualbox_prefix_fix.png` - showing 10.0.0.0/24

## Lessons Learned
Building a lab is not just installing - it's debugging networking fundamentals. Checking VirtualBox NAT settings before configuring Kali saved me hours.

## Reference Structure
Inspired by instructor samples but all IPs, specs and troubleshooting are my own lab on my own laptop.
