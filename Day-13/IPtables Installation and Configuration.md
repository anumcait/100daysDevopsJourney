# Day 13: IPtables Installation and Configuration - KodeKloud 100 Days Challenge

## Overview
In today’s challenge, I focused on enhancing the security of the **Nautilus Infrastructure** running in **Stratos DC**. The security team raised concerns about **Apache’s port 8086** being open for everyone due to the absence of a firewall. To address this, I implemented **iptables** to secure the infrastructure.

### The Requirements:
1. Install **iptables** and all necessary dependencies on each application host.
2. Block incoming traffic on **port 8086** for all hosts except the **Load Balancer (LBR)** host.
3. Ensure that the iptables rules persist across system reboots.

## Step-by-Step Solution

### 1. Install iptables on All App Hosts
I began by installing **iptables** on all application hosts to implement the firewall rules. Depending on the OS, I used the following commands:

#### For RHEL/CentOS/Fedora-based systems:
```bash
sudo yum install iptables-services -y
```
### 2. Configuring iptables Rules

The goal was to block incoming traffic on port 8086 (Apache port) for everyone except the LBR (Load Balancer) host.

- Allow traffic on port 8086 from the LBR host:

```bash
sudo iptables -A INPUT -p tcp -s <LBR_HOST_ADDRESS> --dport 8086 -j ACCEPT
```
- Block all other incoming connections to port 8086:

```bash
sudo iptables -A INPUT -p tcp --dport 8086 -j DROP
```
### 3. Making the iptables Rules Persistent

- To ensure that the firewall rules persist across reboots, I saved the configuration:

For RHEL/CentOS/Fedora systems:
```bash
sudo service iptables save
sudo systemctl enable iptables
```
### 4. Verifying the Configuration

After configuring the iptables rules, I checked that the firewall was correctly applied:
```bash
sudo iptables -L -n -v
```

I confirmed that:

- The LBR host (with IP <LBR_HOST_IP_ADDRESS>) was able to access port 8086.

- All other hosts were blocked from accessing port 8086.

### 5. Testing

Finally, I tested the configuration by trying to access port 8086 from different servers to ensure that the LBR host had access while others were restricted.

## Summary

- Today’s challenge was all about securing the Nautilus infrastructure with iptables. Here’s what I accomplished:

- Installed iptables and set it up on all application hosts.

- Configured firewall rules to allow only the LBR host to access port 8086, blocking all other sources.

- Ensured the firewall rules persist after system reboots.

- This task was a great exercise in firewall management, and I’m feeling more confident in handling security configurations at the system level.
