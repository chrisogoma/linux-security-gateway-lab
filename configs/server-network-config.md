# Server VM Network Configuration

## Purpose

The Server Virtual Machine was configured with a static IP address to represent an internal web server on the Server Network. A default route was configured to forward traffic through the Gateway VM, while DNS servers were specified to enable hostname resolution.

---

## Netplan Configuration

```yaml
network:
  ethernets:
    enp0s3:
      dhcp4: no
      addresses:
        - 172.16.10.10/24
      routes:
        - to: default
          via: 172.16.10.1
      nameservers:
        addresses:
          - 8.8.8.8
          - 1.1.1.1
```

---

## Configuration Explanation

| Setting | Description |
|---------|-------------|
| `ethernets` | Defines the Ethernet network interface. |
| `enp0s3` | Network interface connected to the Server Network. |
| `dhcp4: no` | Disables automatic IPv4 address assignment. |
| `172.16.10.10/24` | Static IP address assigned to the Server VM. |
| `routes` | Defines the routing table. |
| `to: default` | Specifies the default route. |
| `via: 172.16.10.1` | Sets the Gateway VM as the default gateway. |
| `nameservers` | Defines DNS servers used for hostname resolution. |
| `8.8.8.8` | Google Public DNS server. |
| `1.1.1.1` | Cloudflare Public DNS server. |

---

## Result

The Server VM was successfully configured with a static IP address, a default gateway, and public DNS servers. This allowed the Server VM to communicate through the Gateway VM while also resolving external hostnames when required.