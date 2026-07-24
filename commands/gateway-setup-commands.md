# Gateway VM Setup Commands

## Purpose

This document contains the Linux commands used to configure the Gateway Virtual Machine for the Linux Security Gateway Lab project.

Each command is followed by a brief explanation of its purpose so that the setup process can be understood and reproduced.

---

## 1. Check Network Interfaces

```bash
ip a
```

**Purpose:**

Displays all available network interfaces and their assigned IP addresses. This was used to verify that the Gateway VM had two network interfaces configured correctly.

---

## 2. Check Routing Table

```bash
ip route
```

**Purpose:**

Displays the routing table used by the Linux kernel. This was used to verify the default gateway and ensure packets were routed correctly.

---

## 3. Verify IPv4 Forwarding

```bash
cat /proc/sys/net/ipv4/ip_forward
```

**Purpose:**

Checks whether IPv4 forwarding is enabled. A value of `1` indicates that the Gateway VM can forward packets between networks.

---

## 4. Enable IPv4 Forwarding

```bash
sudo sysctl -w net.ipv4.ip_forward=1
```

**Purpose:**

Temporarily enables IPv4 forwarding without requiring a system reboot.

---

## 5. Verify nftables Rules

```bash
sudo nft list ruleset
```

**Purpose:**

Displays the complete nftables firewall configuration currently loaded into the system.

---

## 6. View the Forward Chain

```bash
sudo nft list chain inet filter forward
```

**Purpose:**

Displays all rules configured in the Forward chain, including counters and packet filtering rules.

---

## 7. Create Stateful Firewall Rule

```bash
sudo nft add rule inet filter forward ct state established,related counter accept
```

**Purpose:**

Allows return traffic for already established and related connections while counting matching packets.

---

## 8. Allow HTTP and HTTPS Traffic

```bash
sudo nft add rule inet filter forward \
ip saddr 192.168.10.0/24 \
ip daddr 172.16.10.10 \
tcp dport {80,443} \
counter accept
```

**Purpose:**

Allows HTTP (Port 80) and HTTPS (Port 443) traffic from the Client Network to the Web Server while counting matching packets.

---

## 9. Allow SSH Administration

```bash
sudo nft add rule inet filter forward \
ip saddr 192.168.10.10 \
ip daddr 172.16.10.10 \
tcp dport 22 \
counter accept
```

**Purpose:**

Allows SSH access to the server only from the designated administrator workstation (192.168.10.10).

---

## 10. View Firewall Counters

```bash
sudo nft list chain inet filter forward
```

**Purpose:**

Displays packet and byte counters for each firewall rule to verify that traffic is matching the expected rules.

---

## 11. Test Connectivity

```bash
ping 172.16.10.10
```

**Purpose:**

Verifies basic network connectivity between the Client VM and the Server VM.

---

## 12. Test HTTP Service

```bash
curl http://172.16.10.10
```

**Purpose:**

Confirms that the web server is reachable through the firewall and responds to HTTP requests.

---

## 13. Test HTTPS Service

```bash
curl -k https://172.16.10.10
```

**Purpose:**

Confirms that HTTPS traffic is successfully forwarded through the Gateway firewall.

---

## 14. Test SSH Access

```bash
ssh chrissytech@172.16.10.10
```

**Purpose:**

Verifies that SSH administration is permitted only from the authorized administrator workstation.

---

## Summary

The commands documented above were used to:

- Configure the Linux Gateway VM.
- Verify network connectivity.
- Enable packet forwarding.
- Configure a stateful nftables firewall.
- Allow approved HTTP, HTTPS, and SSH traffic.
- Validate firewall functionality through service testing.
- Confirm packet matching using firewall counters.

These commands formed the core configuration process for the Linux Security Gateway Lab.