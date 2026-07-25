# Client VM Setup Commands

## Purpose

This document contains the Linux commands used to configure the Client Virtual Machine for the Linux Security Gateway Lab.

Each command is followed by a brief explanation describing its purpose.

---

# 1. Navigate to the Netplan Configuration Directory

```bash
cd /etc/netplan
```

**Purpose**

Changes the current directory to the location where Ubuntu stores its network configuration files.

---

# 2. Display Available Configuration Files

```bash
ls
```

**Purpose**

Lists the files in the Netplan directory to identify the network configuration file.

---

# 3. View the Current Network Configuration

```bash
sudo cat /etc/netplan/00-installer-config.yaml
```

**Purpose**

Displays the contents of the Netplan configuration file without opening an editor.

This command was used to verify the Client VM network settings.

---

# 4. Edit the Network Configuration

```bash
sudo nano /etc/netplan/00-installer-config.yaml
```

**Purpose**

Opens the Netplan configuration file for editing using the Nano text editor.

Static IP addressing and the default route were configured here.

---

# 5. Apply the Network Configuration

```bash
sudo netplan apply
```

**Purpose**

Applies the updated Netplan configuration immediately without restarting the virtual machine.

---

# 6. Verify the Assigned IP Address

```bash
ip a
```

**Purpose**

Displays all network interfaces and confirms that the Client VM received the correct static IP address.

---

# 7. Verify the Routing Table

```bash
ip route
```

**Purpose**

Displays the routing table to verify that the default gateway points to the Gateway VM.

---

# 8. Test Connectivity to the Gateway

```bash
ping 192.168.10.1
```

**Purpose**

Confirms that the Client VM can successfully communicate with the Gateway VM.

---

# Result

The Client VM was successfully configured with a static IP address and a default gateway, allowing communication with the Gateway VM and enabling traffic to be forwarded to the Server Network through the Linux Security Gateway.