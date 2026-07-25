# Client VM Network Configuration

## Purpose

The Client Virtual Machine was configured with a static IP address to represent a workstation on the Client Network. A default route was also configured so that all traffic destined for external networks is forwarded to the Gateway VM for routing and firewall inspection.

---

## Netplan Configuration

```yaml
network:
  version: 2
  ethernets:
    enp0s3:
      dhcp: no
      addresses:
        - 192.168.10.10/24
      routes:
        - to: default
          via: 192.168.10.1
```

---

## Configuration Explanation

| Setting | Description |
|---------|-------------|
| `version: 2` | Specifies the Netplan configuration format. |
| `ethernets` | Defines the Ethernet network interface. |
| `enp0s3` | Network interface connected to the Client Network. |
| `dhcp: no` | Disables automatic IP address assignment (DHCP). |
| `192.168.10.10/24` | Static IP address assigned to the Client VM. |
| `routes` | Defines the routing table for the Client VM. |
| `to: default` | Specifies the default route for all traffic leaving the local network. |
| `via: 192.168.10.1` | Sets the Gateway VM as the default gateway. |

---

## Result

The Client VM successfully communicated with the Gateway VM using a static IP address. All traffic intended for external networks was forwarded to the Gateway (`192.168.10.1`), allowing the firewall to inspect and control network communication between the Client Network and the Server Network.