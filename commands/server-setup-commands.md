# Server VM Setup Commands

## Purpose

This document contains the Linux commands used to configure the Server Virtual Machine for the Linux Security Gateway Lab.

Each command is accompanied by a brief explanation.

---

# 1. Navigate to the Netplan Directory

```bash
cd /etc/netplan
```

**Purpose**

Moves to the directory containing Ubuntu's network configuration files.

---

# 2. Display Available Configuration Files

```bash
ls
```

**Purpose**

Lists the available Netplan configuration files.

---

# 3. View the Current Network Configuration

```bash
sudo cat /etc/netplan/00-installer-config.yaml
```

**Purpose**

Displays the Server VM network configuration.

---

# 4. Edit the Network Configuration

```bash
sudo nano /etc/netplan/00-installer-config.yaml
```

**Purpose**

Opens the Netplan configuration file for editing.

---

# 5. Apply the Configuration

```bash
sudo netplan apply
```

**Purpose**

Applies the updated network configuration immediately.

---

# 6. Verify the Assigned IP Address

```bash
ip a
```

**Purpose**

Verifies that the Server VM received the expected static IP address.

---

# 7. Verify the Routing Table

```bash
ip route
```

**Purpose**

Confirms that the Gateway VM is configured as the default gateway.

---

# 8. Test Connectivity to the Gateway

```bash
ping 172.16.10.1
```

**Purpose**

Confirms communication between the Server VM and the Gateway VM.

---

# Result

The Server VM was successfully configured with a static IP address, default gateway, and DNS servers, enabling communication through the Linux Security Gateway.