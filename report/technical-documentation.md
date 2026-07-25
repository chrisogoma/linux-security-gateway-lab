# Linux Security Gateway Lab

## Technical Implementation Report

---

# 1. Executive Summary

The Linux Security Gateway Lab demonstrates the implementation of a Linux-based security gateway using Ubuntu Server and the **nftables** firewall framework within a virtualized environment. The objective of the project was to design a segmented network in which all communication between a Client Network and a Server Network passes through a dedicated Gateway Virtual Machine responsible for routing, filtering, and logging network traffic.

The laboratory environment consisted of three virtual machines deployed using Oracle VirtualBox:

- Gateway Virtual Machine
- Client Virtual Machine
- Server Virtual Machine

Static IP addressing was configured using Ubuntu Netplan to ensure predictable communication between systems. The Gateway Virtual Machine was configured with two network interfaces, enabling it to route traffic between the Client Network (`192.168.10.0/24`) and the Server Network (`172.16.10.0/24`).

A stateful firewall was implemented using **nftables** following a **default-deny security model**. Only approved services, including HTTP, HTTPS, and authorized Secure Shell (SSH) administration, were permitted between the two networks. Invalid packets and unauthorized access attempts were blocked and logged to provide visibility into suspicious activity.

Following implementation, a series of validation tests confirmed that:

- Static network configuration operated correctly.
- Routing between networks functioned as expected.
- Approved services were successfully permitted.
- Unauthorized traffic was blocked.
- Firewall logging operated correctly.

Throughout the project, Git and GitHub were used to maintain version control and organize technical documentation. Commands, configuration files, architecture diagrams, and implementation evidence were documented separately to improve maintainability and reproducibility.

The completed project demonstrates practical skills in Linux system administration, network routing, firewall implementation, technical documentation, and version control while providing a reproducible example of a secure Linux gateway suitable for a small enterprise network simulation.

---

# 2. Objectives and Assumptions

## Objectives

The primary objective of this project was to design, implement, and validate a Linux-based Security Gateway capable of securely routing traffic between isolated networks while enforcing access control through a stateful firewall.

The project was designed to demonstrate practical skills in Linux system administration, networking, firewall implementation, and technical documentation.

The specific objectives were to:

- Design a segmented network consisting of a Client Network and a Server Network.
- Configure a Linux Gateway Virtual Machine with two network interfaces to route traffic between both networks.
- Configure static IP addressing for all virtual machines using Ubuntu Netplan.
- Enable packet forwarding through the Gateway Virtual Machine.
- Implement a stateful firewall using the **nftables** framework.
- Allow only approved network services between the Client and Server networks.
- Block and log unauthorized network traffic.
- Validate the implementation using connectivity and firewall testing.
- Document the implementation process using Git, GitHub, and Markdown.

## Assumptions

The implementation of this project was based on the following assumptions:

- The laboratory environment was deployed using Oracle VirtualBox.
- All virtual machines communicated using private IPv4 networks.
- Static IP addressing was used throughout the environment.
- The Gateway Virtual Machine served as the only routing device.
- All communication between networks passed through the Gateway.
- The Server VM simulated an internal enterprise server.
- The Client VM simulated an internal workstation.
- **nftables** enforced all firewall policies.
- Git and GitHub were used for documentation and version control.

---

# 3. Lab Architecture

## Overview

The laboratory environment was designed to simulate a small enterprise network consisting of two isolated network segments connected through a dedicated Linux Security Gateway.

The Gateway Virtual Machine performs both routing and firewall functions, ensuring that all communication between the Client Network and the Server Network is inspected before being forwarded.

## Virtual Machine Roles

| Virtual Machine | Operating System | Role |
|-----------------|------------------|------|
| Gateway VM | Ubuntu Server | Routing and Firewall |
| Client VM | Ubuntu Desktop | Internal Workstation |
| Server VM | Ubuntu Server | Internal Web Server |

## Network Addressing Scheme

| Network | Address Range | Gateway |
|----------|---------------|---------|
| Client Network | 192.168.10.0/24 | 192.168.10.1 |
| Server Network | 172.16.10.0/24 | 172.16.10.1 |

### Virtual Machine Addresses

| Device | IP Address |
|---------|------------|
| Gateway VM (Client Interface) | 192.168.10.1 |
| Gateway VM (Server Interface) | 172.16.10.1 |
| Client VM | 192.168.10.10 |
| Server VM | 172.16.10.10 |

## Network Topology

**Figure 1 – Linux Security Gateway Lab Architecture**

![Network Architecture](../diagrams/network-architecture.png)

## Design Considerations

The architecture was designed using the following principles:

- Network Segmentation
- Centralized Traffic Inspection
- Static Addressing
- Stateful Packet Filtering
- Default-Deny Firewall Policy

These design decisions create a secure communication path between isolated networks while maintaining centralized control over network traffic.


---

# 4. Build and Configuration Process

## 4.1 Environment Preparation

The laboratory environment was deployed using Oracle VirtualBox. Three virtual machines were created to simulate a small enterprise network consisting of a Gateway, Client, and Server.

Each virtual machine was assigned a dedicated role within the environment to ensure clear separation of responsibilities.

The virtual machines deployed were:

- Gateway Virtual Machine
- Client Virtual Machine
- Server Virtual Machine

After installation, each operating system was updated and verified to ensure that all required networking components were available before configuration began.

---

## 4.2 Gateway Virtual Machine Configuration

The Gateway Virtual Machine was configured first because it forms the foundation of the entire laboratory environment.

Two network interfaces were assigned to the Gateway:

- One interface connected to the Client Network.
- One interface connected to the Server Network.

Static IP addresses were configured using Ubuntu Netplan.

| Interface | Address |
|-----------|---------|
| enp0s8 | 192.168.10.1/24 |
| enp0s3 | 172.16.10.1/24 |

IPv4 packet forwarding was then enabled, allowing the Gateway to route packets between both networks.

The complete configuration is documented in:

```text
configs/gateway-network-config.md
```

The implementation commands are documented in:

```text
commands/gateway-setup-commands.md
```

**Evidence**

![Gateway Configuration](../screenshots/Task%201%20pic%201.png)

---

## 4.3 Client Virtual Machine Configuration

The Client Virtual Machine represents an internal workstation connected to the Client Network.

A static IP address was configured using Ubuntu Netplan.

| Setting | Value |
|---------|-------|
| Address | 192.168.10.10/24 |
| Default Gateway | 192.168.10.1 |

The default gateway ensures that all traffic destined for external networks passes through the Linux Gateway before reaching the Server Network.

The complete configuration is available in:

```text
configs/client-network-config.md
```

The implementation commands are available in:

```text
commands/client-setup-commands.md
```

**Evidence**

![Client Configuration](../screenshots/Task%202%20pic%201.png)

---

## 4.4 Server Virtual Machine Configuration

The Server Virtual Machine represents an internal enterprise server.

A static IP address and public DNS servers were configured using Ubuntu Netplan.

| Setting | Value |
|---------|-------|
| Address | 172.16.10.10/24 |
| Default Gateway | 172.16.10.1 |
| DNS | 8.8.8.8, 1.1.1.1 |

The Server communicates with the Client Network only through the Gateway Virtual Machine.

The complete configuration is documented in:

```text
configs/server-network-config.md
```

The implementation commands are documented in:

```text
commands/server-setup-commands.md
```

**Evidence**

![Server Configuration](../screenshots/Task%203%20pic%201.png)

---

## 4.5 Network Verification

After configuring all virtual machines, several verification steps were performed.

The following items were confirmed:

- Correct interface configuration.
- Static IP addressing.
- Correct routing tables.
- Successful communication between virtual machines.

These tests confirmed that the network infrastructure had been correctly deployed before implementing firewall rules.

---

## 4.6 Repository Documentation

Throughout the implementation process, Git and GitHub were used for version control and documentation.

The repository was organized into dedicated directories containing:

- Commands
- Configurations
- Diagrams
- Screenshots
- Technical Documentation

This organization improves maintainability and enables another administrator to reproduce the environment.

---

# 5. nftables Security Policy

## 5.1 Overview

The Linux Security Gateway implements a stateful firewall using the **nftables** framework.

The firewall follows a **default-deny** security model, meaning that all traffic is blocked unless explicitly permitted.

The firewall was designed to:

- Permit legitimate communication.
- Deny unauthorized access.
- Log suspicious traffic.
- Maintain connection awareness through stateful inspection.

---

## 5.2 Firewall Design

The firewall consists of three filtering chains.

| Chain | Purpose | Default Policy |
|--------|----------|----------------|
| Input | Protects the Gateway | Drop |
| Forward | Controls traffic between networks | Drop |
| Output | Controls Gateway outbound traffic | Accept |

---

## 5.3 Stateful Packet Filtering

Stateful packet filtering allows the firewall to recognise existing communication sessions.

The firewall accepts packets belonging to:

- Established connections.
- Related connections.

Packets classified as **Invalid** are immediately discarded.

This approach improves both security and firewall performance.

---

## 5.4 Approved Services

Only the following services were permitted.

| Service | Protocol | Port |
|----------|----------|------|
| HTTP | TCP | 80 |
| HTTPS | TCP | 443 |
| SSH | TCP | 22 (Authorised Client Only) |

All remaining services were denied by default.

---

## 5.5 Traffic Restriction

Traffic was denied whenever it failed to satisfy an approved firewall rule.

Blocked traffic included:

- Unauthorized SSH connections.
- Unexpected forwarded packets.
- Unknown protocols.
- Invalid packet states.

This follows the Principle of Least Privilege by exposing only the minimum services required.

---

## 5.6 Logging

Firewall logging was enabled to record:

- Unauthorized SSH attempts.
- Packets denied by the Input Chain.
- Packets denied by the Forward Chain.

Rate limiting was implemented to prevent excessive log generation.

---

## 5.7 Firewall Summary

The implemented firewall successfully provides:

- Network segmentation.
- Least-privilege access control.
- Stateful inspection.
- Administrative access restriction.
- Security logging.
- Default-deny protection.

The complete firewall implementation is documented in:

```text
configs/nftables-firewall-config.md
```

**Evidence**

![nftables Rules](../screenshots/Task%204.png)

---

# 6. Testing Approach and Results

## 6.1 Testing Strategy

Following implementation, validation testing was performed to confirm that the Linux Security Gateway behaved according to the intended design.

Testing focused on:

- Network configuration.
- Connectivity.
- Routing.
- Firewall enforcement.
- Approved services.
- Logging.

Each completed task was documented using screenshots stored within the repository.

---

## 6.2 Network Configuration Validation

The first phase confirmed:

- Static IP configuration.
- Correct Netplan deployment.
- Proper routing configuration.
- Successful interface initialization.

**Evidence**

![Task 1](../screenshots/Task%201%20pic%202.png)

---

## 6.3 Connectivity Testing

Connectivity testing confirmed successful communication between:

- Client → Gateway
- Server → Gateway
- Client → Server (through the Gateway)

These tests verified that routing and packet forwarding were functioning correctly.

**Evidence**

![Task 2](../screenshots/Task%202%20pic%202.png)

---

## 6.4 Firewall Validation

The nftables firewall was validated by confirming that:

- HTTP traffic was permitted.
- HTTPS traffic was permitted.
- Authorized SSH access was permitted.
- Unauthorized traffic was denied.

The firewall correctly enforced the configured security policy.

**Evidence**

![Task 5](../screenshots/Task%205%20pic%201.png)

---

## 6.5 Logging Validation

Firewall logging successfully recorded unauthorized connection attempts before dropping the packets.

This confirmed that logging and monitoring features operated correctly.

---

## 6.6 Overall Results

Testing confirmed that all implementation objectives were successfully achieved.

The Linux Security Gateway successfully:

- Routed traffic between isolated networks.
- Implemented stateful packet inspection.
- Allowed approved services.
- Blocked unauthorized communication.
- Logged suspicious activity.
- Maintained reliable communication between all virtual machines.

These results demonstrate that the implemented Linux Security Gateway functions as intended and provides secure communication between segmented network environments.

---

---

# 7. Troubleshooting Exercise

## 7.1 Overview

During the implementation of the Linux Security Gateway Lab, several technical challenges were encountered while configuring the virtual machines, implementing routing, configuring the firewall, and maintaining the GitHub repository.

Each issue was investigated systematically by identifying the root cause, applying corrective actions, validating the solution, and documenting the outcome. This troubleshooting process strengthened practical knowledge of Linux networking, firewall management, and version control.

---

## 7.2 Network Configuration Issues

### Problem

Initially, communication between the virtual machines was unsuccessful despite assigning static IP addresses.

### Cause

The Netplan configuration required adjustments, and IPv4 packet forwarding had not yet been enabled on the Gateway Virtual Machine.

### Resolution

The network configuration files were reviewed and corrected. Static IP addresses were assigned using Netplan, after which the configuration was applied.

IPv4 packet forwarding was then enabled on the Gateway Virtual Machine, allowing traffic to be routed successfully between the Client and Server networks.

### Outcome

Communication between all virtual machines was successfully established, confirming that routing had been correctly configured.

---

## 7.3 nftables Configuration Issues

### Problem

During firewall implementation, syntax errors occurred while creating and verifying nftables rules.

### Cause

Incorrect command syntax and formatting prevented nftables from accepting certain commands.

### Resolution

The command syntax was reviewed and corrected using the official nftables documentation.

After correcting the syntax, the firewall rules were successfully created and verified.

### Outcome

The firewall rules were implemented successfully and functioned according to the intended security policy.

---

## 7.4 Virtual Machine Networking

### Problem

During Ubuntu Server installation, package downloads failed while the virtual machine was connected only to an Internal Network.

### Cause

The virtual machine had no Internet access during installation.

### Resolution

The network adapter was temporarily changed to **NAT**, allowing Ubuntu to download the required packages.

Once installation was completed, the adapter configuration was restored to the intended laboratory network topology.

### Outcome

Ubuntu Server installation completed successfully without affecting the final network design.

---

## 7.5 Git and GitHub Challenges

### Problem

Several issues were encountered while maintaining the GitHub repository.

Examples included:

- Push failures caused by missing upstream branches.
- Images appearing locally but not displaying correctly on GitHub.
- Markdown rendering issues.
- Repository synchronization problems.

### Resolution

Git diagnostic commands were used to identify the source of each problem.

Repository synchronization was restored by correctly staging, committing, and pushing changes.

Markdown rendering issues were resolved by ensuring files used the `.md` extension and by following GitHub Flavored Markdown syntax.

### Outcome

The repository was successfully synchronized, and all project documentation, screenshots, diagrams, commands, and configuration files became available through GitHub.

---

## 7.6 Documentation Challenges

### Problem

Initially, project documentation lacked a consistent structure, making it difficult to organize commands, configurations, screenshots, and implementation evidence.

### Resolution

The repository was reorganized into dedicated directories containing:

- Commands
- Configurations
- Diagrams
- Screenshots
- Technical Documentation

This modular structure improved readability, maintainability, and reproducibility.

### Outcome

The completed repository now provides a clear and well-organized implementation guide that can easily be followed by another administrator.

---

## 7.7 Troubleshooting Summary

The troubleshooting process significantly improved practical skills in:

- Linux system administration
- Network configuration
- Routing
- Firewall implementation
- Git version control
- Technical documentation
- Structured problem solving

Each issue encountered during implementation contributed to a deeper understanding of Linux networking and enterprise security practices.

---

# 8. Security Observations and Limitations

## 8.1 Security Observations

The Linux Security Gateway successfully demonstrates several important security principles commonly used within enterprise environments.

### Network Segmentation

The Client Network and Server Network were isolated using separate IPv4 subnets.

This segmentation ensures that communication between both networks occurs only through the Gateway Virtual Machine, allowing security policies to be enforced before traffic reaches its destination.

Network segmentation reduces the attack surface and limits lateral movement should one segment become compromised.

---

### Principle of Least Privilege

The implemented firewall follows the Principle of Least Privilege by allowing only the minimum services required for operation.

Approved services include:

- HTTP
- HTTPS
- Authorized SSH administration

All other network traffic is denied by default.

---

### Stateful Firewall Protection

The firewall implements stateful packet inspection.

Rather than evaluating every packet independently, nftables tracks active communication sessions and automatically permits packets belonging to established or related connections.

This improves both security and firewall efficiency.

---

### Administrative Access Control

SSH access was restricted to the authorized Client Virtual Machine.

Restricting administrative access to a trusted source reduces the likelihood of unauthorized remote administration attempts.

---

### Firewall Logging

Logging was configured to record unauthorized traffic and blocked connection attempts.

These logs provide administrators with useful information during troubleshooting and future security investigations.

---

## 8.2 Limitations

Although the Linux Security Gateway successfully demonstrates the core concepts of Linux routing and firewall implementation, several limitations remain.

### Single Point of Failure

The Gateway Virtual Machine represents the only routing and security device within the environment.

Failure of the Gateway immediately prevents communication between both networks.

Enterprise environments typically mitigate this through redundant gateways and high-availability solutions.

---

### Basic Firewall Policy

The firewall performs filtering primarily based on:

- IP addresses
- Protocols
- Connection state
- Destination ports

More advanced capabilities such as:

- Deep Packet Inspection (DPI)
- Intrusion Detection Systems (IDS)
- Intrusion Prevention Systems (IPS)
- Application-aware filtering

were intentionally excluded from the scope of this laboratory.

---

### Limited Monitoring

Although firewall logging was successfully implemented, the environment does not include centralized monitoring solutions such as Security Information and Event Management (SIEM) platforms.

Enterprise environments commonly integrate firewall logs into centralized monitoring systems for real-time detection and incident response.

---

### Small Network Scope

The laboratory consists of only three virtual machines.

While appropriate for demonstrating Linux networking concepts, larger enterprise environments typically include:

- Multiple VLANs
- Domain Controllers
- Database Servers
- Multiple Client Networks
- Cloud Infrastructure
- Redundant Network Devices

Supporting larger environments would require additional routing, firewall, and monitoring considerations.

---

## 8.3 Future Improvements

The following enhancements could further improve the security capabilities of the environment:

- Deploy an Intrusion Detection System (IDS) such as Suricata or Snort.
- Integrate centralized logging using a SIEM platform.
- Implement VLAN segmentation.
- Deploy redundant Gateway Virtual Machines.
- Implement VPN connectivity for secure remote administration.
- Expand firewall rules to include application-layer filtering.
- Automate firewall deployment using Infrastructure as Code (IaC).

---

## Security Assessment Summary

The Linux Security Gateway successfully demonstrates the practical application of:

- Network segmentation
- Stateful packet inspection
- Least-privilege access control
- Controlled administrative access
- Firewall logging

Although simplified for educational purposes, the implementation provides a strong foundation for understanding Linux-based network security.

---

# 9. Lessons Learned

The Linux Security Gateway Lab provided practical experience in designing, implementing, securing, and documenting a Linux-based network environment.

One of the most valuable lessons learned was the importance of **network segmentation**. Separating the Client and Server networks demonstrated how enterprise environments isolate resources to improve security and reduce the attack surface.

The project also reinforced the importance of **static network configuration**. Configuring interfaces, routing tables, and default gateways using Ubuntu Netplan demonstrated how accurate network planning directly impacts reliable communication between systems.

Implementing **stateful firewall policies** using nftables provided practical experience in modern firewall technologies. Rather than simply allowing or denying packets, the firewall tracked active communication sessions and differentiated between established, related, and invalid traffic.

Troubleshooting formed a significant part of the implementation process. Configuration errors, routing issues, firewall syntax problems, and GitHub synchronization challenges required systematic investigation before appropriate solutions could be applied. These experiences emphasized the importance of structured problem-solving within cybersecurity and systems administration.

The project also highlighted the importance of **technical documentation**. Organizing commands, configurations, screenshots, and implementation evidence into separate sections improved maintainability and created documentation that can be reused by other administrators.

Using Git and GitHub throughout the project introduced practical version control practices widely used within professional software engineering and cybersecurity environments.

Overall, the Linux Security Gateway Lab strengthened practical skills in:

- Linux administration
- Networking
- Routing
- Firewall implementation
- Troubleshooting
- Version control
- Technical documentation

Most importantly, the project demonstrated that effective cybersecurity depends not only on implementing security controls but also on understanding, documenting, validating, and maintaining those controls throughout the system lifecycle.

---

---

# 10. References

The following resources were consulted during the implementation and documentation of this project.

1. Canonical Ltd. (2026). *Netplan Documentation*. Available at: https://netplan.io/

2. Canonical Ltd. (2026). *Ubuntu Server Documentation*. Available at: https://ubuntu.com/server/docs

3. The Netfilter Project. (2026). *nftables Documentation*. Available at: https://wiki.nftables.org/

4. Oracle Corporation. (2026). *Oracle VM VirtualBox User Manual*. Available at: https://www.virtualbox.org/manual/

5. Git Project. (2026). *Git Documentation*. Available at: https://git-scm.com/doc

6. GitHub Inc. (2026). *GitHub Documentation*. Available at: https://docs.github.com/

7. Internet Engineering Task Force (IETF). (1981). *RFC 791 – Internet Protocol*. Available at: https://www.rfc-editor.org/rfc/rfc791

8. Internet Engineering Task Force (IETF). (1981). *RFC 793 – Transmission Control Protocol*. Available at: https://www.rfc-editor.org/rfc/rfc793

9. Ubuntu Community Documentation. (2026). *Ubuntu Networking*. Available at: https://help.ubuntu.com/

10. Oracle Corporation. (2026). *VirtualBox Networking Guide*. Available at: https://www.virtualbox.org/

---

# 11. AI and External Assistance Disclosure

## AI and External Assistance Disclosure

Artificial Intelligence (AI) tools were used during the preparation of this project's technical documentation to improve clarity, organization, grammar, formatting, and readability.

AI assistance was specifically used for:

- Improving technical writing.
- Organizing the report into a professional structure.
- Refining explanations of Linux networking concepts.
- Improving grammar and formatting.
- Assisting with Markdown documentation for the accompanying GitHub repository.

All practical implementation activities—including installation, configuration, routing, firewall implementation, troubleshooting, validation, testing, and repository management—were completed by the project author.

All AI-assisted content was carefully reviewed and verified before inclusion in the final documentation to ensure that it accurately reflects the implemented laboratory environment.

---

# 12. Appendices

## Appendix A – Repository Structure

The project repository is organized into multiple directories to improve readability, maintainability, and reproducibility.

```text
linux-security-gateway-lab/
│
├── README.md
│
├── commands/
│   ├── gateway-setup-commands.md
│   ├── client-setup-commands.md
│   └── server-setup-commands.md
│
├── configs/
│   ├── gateway-network-config.md
│   ├── client-network-config.md
│   ├── server-network-config.md
│   └── nftables-firewall-config.md
│
├── diagrams/
│   └── network-architecture.png
│
├── screenshots/
│   ├── Task 1 pic 1.png
│   ├── Task 1 pic 2.png
│   ├── Task 2 pic 1.png
│   ├── Task 2 pic 2.png
│   ├── Task 2 pic 3.png
│   ├── Task 3 pic 1.png
│   ├── Task 3 pic 2.png
│   ├── Task 4.png
│   ├── Task 5 pic 1.png
│   ├── ...
│   └── Task 11.png
│
└── report/
    └── technical-documentation.md
```

---

## Appendix B – Configuration Files

The following configuration files are included within the repository.

| Configuration | Location |
|--------------|----------|
| Gateway Network Configuration | `configs/gateway-network-config.md` |
| Client Network Configuration | `configs/client-network-config.md` |
| Server Network Configuration | `configs/server-network-config.md` |
| nftables Firewall Configuration | `configs/nftables-firewall-config.md` |

---

## Appendix C – Implementation Commands

All implementation commands used throughout the project have been documented separately.

| Command Documentation | Location |
|----------------------|----------|
| Gateway Commands | `commands/gateway-setup-commands.md` |
| Client Commands | `commands/client-setup-commands.md` |
| Server Commands | `commands/server-setup-commands.md` |

---

## Appendix D – Network Architecture

The complete laboratory network topology diagram is available within the repository.

| Resource | Location |
|----------|----------|
| Network Architecture Diagram | `diagrams/network-architecture.png` |

---

## Appendix E – Validation Evidence

Implementation evidence was collected throughout the project and organized according to individual implementation tasks.

The screenshots demonstrate:

- Network interface configuration.
- Static IP configuration.
- Netplan verification.
- Routing validation.
- Packet forwarding.
- nftables implementation.
- Firewall validation.
- Connectivity testing.
- Service validation.
- Logging verification.
- Final implementation.

All screenshots are available within:

```text
screenshots/
```

---

## Appendix F – Project Deliverables

The completed project repository contains the following deliverables:

- Technical Documentation
- Linux Gateway Configuration
- Client Configuration
- Server Configuration
- nftables Firewall Configuration
- Network Architecture Diagram
- Command Documentation
- Validation Screenshots
- Git Version History

These deliverables collectively provide sufficient information for another administrator to reproduce the Linux Security Gateway environment.

---

# Conclusion

The Linux Security Gateway Lab successfully demonstrated the implementation of a secure Linux-based routing and firewall solution using Ubuntu Server and the nftables framework.

The project achieved its primary objectives by implementing a segmented network architecture, configuring static routing, enabling packet forwarding, enforcing a stateful firewall policy, validating approved services, and documenting the complete implementation using Git and GitHub.

Beyond the technical implementation, the project emphasized the importance of structured troubleshooting, professional documentation, and version control in modern systems administration and cybersecurity practice.

The completed repository serves not only as evidence of the successful completion of the laboratory but also as a reusable reference for future Linux networking and firewall projects.

---
