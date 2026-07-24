# Gateway Network Configuration

## Purpose

The Gateway Virtual Machine was configured using Ubuntu Netplan with two network interfaces. Static IP addresses were assigned to enable routing between the Client Network and the Server Network.

---

## Netplan Configuration

```yaml
network:
 version: 2
   ethernets:
    enp0s3:
      dhcp: no
      addresses:
        - 172.16.10.1/24

    enp0s8:
      addresses:
        - 192.168.10.1/24
```

---

## Configuration Explanation

| Setting | Description |
|---------|-------------|
| `version: 2` | Specifies the Netplan configuration format. |
| `ethernets` | Defines Ethernet network interfaces. |
| `enp0s3` | Gateway interface connected to the Server Network. |
| `enp0s8` | Gateway interface connected to the Client Network. |
| `dhcp: no` | Disables automatic IP address assignment on `enp0s3`. |
| `172.16.10.1/24` | Static IP address assigned to the Server-facing interface. |
| `192.168.10.1/24` | Static IP address assigned to the Client-facing interface. |

---

## Result

This configuration allowed the Gateway VM to communicate with both private networks and provided the routing foundation required for implementing the nftables firewall.