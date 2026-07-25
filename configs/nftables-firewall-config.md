# nftables Firewall Configuration

## Purpose

The Gateway Virtual Machine uses **nftables** as a stateful firewall to control traffic flowing between the Client Network and the Server Network.

The firewall follows the principle of **default deny**, allowing only explicitly approved traffic while dropping and logging unauthorized packets.

---

## Firewall Ruleset

```nft
table inet filter {

    chain input {
        type filter hook input priority filter; policy drop;

        ct state invalid counter packets 0 bytes 0 drop
        ct state established,related counter packets 365 bytes 27177 accept
        iif "lo" counter packets 123 bytes 3127 accept
        ip saddr 192.168.10.10 tcp dport 22 counter packets 0 bytes 0 accept
        icmp type echo-request counter packets 0 bytes 0 accept

        counter packets 0 bytes 0 log prefix "nft-input-drop: " limit rate 5/minute burst 5 packets drop
    }

    chain forward {
        type filter hook forward priority filter; policy drop;

        ct state invalid counter packets 0 bytes 0 drop
        ct state established,related counter packets 0 bytes 0 accept
        ip saddr 192.168.10.10 ip daddr 172.16.10.10 tcp dport 22 counter packets 0 bytes 0 accept
        ip saddr 192.168.10.0/24 ip daddr 172.16.10.10 tcp dport {80,443} counter packets 0 bytes 0 accept
        ip daddr 172.16.10.10 tcp dport 22 counter packets 0 bytes 0 log prefix "nft-block-ssh: " limit rate 5/minute burst 5 packets drop

        counter packets 0 bytes 0 log prefix "nft-block-drop: " limit rate 5/minute burst 5 packets drop
    }

    chain output {
        type filter hook output priority filter; policy accept;
    }
}
```

---

# Security Policy Implemented

| Traffic | Action |
|---------|--------|
| Established and related connections | Allowed |
| Invalid packets | Dropped |
| Loopback interface | Allowed |
| ICMP Echo Requests (Ping) | Allowed |
| SSH from approved Client VM | Allowed |
| HTTP (Port 80) | Allowed |
| HTTPS (Port 443) | Allowed |
| Unauthorized SSH attempts | Logged and dropped |
| All other forwarded traffic | Logged and dropped |
| Outbound traffic | Allowed |

---

# Rule Explanation

### Input Chain

The Input Chain protects the Gateway itself.

The default policy is **Drop**, meaning any traffic not explicitly allowed is denied.

Allowed traffic includes:

- Established and related connections.
- Loopback traffic.
- SSH from the approved Client VM.
- ICMP Echo Requests.

Invalid packets are immediately discarded.

---

### Forward Chain

The Forward Chain controls traffic travelling through the Gateway.

The default policy is **Drop**.

The following traffic is permitted:

- HTTP (Port 80)
- HTTPS (Port 443)
- SSH from the approved Client VM to the Server VM.

All other forwarded traffic is logged and dropped.

---

### Output Chain

The Output Chain uses the default **Accept** policy.

This allows the Gateway to initiate outbound communication when necessary.

---

# Logging

The firewall generates logs for:

- Blocked SSH attempts
- Dropped forwarded traffic
- Dropped input traffic

Rate limiting is applied to prevent excessive log generation.

---

# Result

The implemented firewall successfully enforced a least-privilege security policy by allowing only approved traffic while blocking and logging unauthorized access attempts between the Client and Server networks.