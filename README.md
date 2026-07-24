# Linux Security Gateway Lab

## Project Overview

This project demonstrates the design, configuration, and implementation of a Linux-based Security Gateway using Ubuntu Server and **nftables**.

The Gateway acts as both a router and a firewall between a Client Network and a Server Network. The project implements stateful packet filtering, controlled network access, traffic logging, and validation of firewall rules.

This lab was completed as part of a Linux Security Gateway assignment to demonstrate practical Linux administration and network security skills.

---

## Objectives

- Configure a Linux Gateway using Ubuntu Server.
- Enable communication between isolated networks.
- Implement a stateful firewall using nftables.
- Allow only approved services.
- Block unauthorized traffic.
- Log firewall activity.
- Validate firewall functionality.

---

## Lab Architecture

The project consists of three virtual machines:

- **Gateway VM**
- **Client VM**
- **Server VM**

The Gateway routes and filters traffic between the Client Network (`192.168.10.0/24`) and the Server Network (`172.16.10.0/24`).

Network architecture diagram:

`diagrams/network-architecture.png`

---

## Technologies Used

- Ubuntu Server
- Ubuntu Desktop
- Kali Linux
- VirtualBox
- nftables
- Netplan
- Git
- GitHub
- Markdown

---

## Repository Structure

```text
linux-security-gateway-lab/
│
├── commands/
├── configs/
├── diagrams/
├── report/
├── screenshots/
└── README.md
```

---

## Project Tasks

The project includes:

- Network design
- Gateway configuration
- Client configuration
- Server configuration
- Basic connectivity testing
- Stateful firewall implementation
- Approved access configuration
- Restricted access implementation
- Logging and counters
- Service validation

Supporting screenshots are available in the `screenshots` folder.

---

## Configuration Files

Configuration documentation is located in the `configs` folder and includes:

- Gateway Network Configuration
- Server Network Configuration
- Client Network Configuration
- nftables Firewall Configuration

---

## Screenshots

Evidence of every completed task is available inside the `screenshots` directory.

Each screenshot has been labelled according to the corresponding task.

---

## Key Learning Outcomes

Through this project, I gained practical experience in:

- Linux networking
- Static IP configuration
- Network routing
- Stateful firewall implementation
- nftables rule creation
- Git and GitHub workflow
- Technical documentation using Markdown

---

## Author

**Christine Ogoma Nwaorgu**

Software Engineer | Cybersecurity Enthusiast | Cloud Security Learner