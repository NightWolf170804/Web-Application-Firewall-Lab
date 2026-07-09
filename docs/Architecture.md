# Architecture

## Overview

This document describes the architecture of the SafeLine WAF + DVWA Security Lab.

The objective of this lab is to demonstrate how a Web Application Firewall (WAF) can protect a vulnerable web application by inspecting, filtering, and forwarding HTTP/HTTPS traffic before it reaches the backend server.

---

# Architecture Diagram

![SafeLine WAF + DVWA Lab Architecture](../images/architecture.png)

---

# Architecture Description

The lab consists of two virtual machines connected using **VMware Bridged Networking**.

### Ubuntu VM (Server)

The Ubuntu virtual machine hosts all the backend services required for the lab.

Components:

- SafeLine WAF (Docker)
- Apache Web Server
- PHP
- DVWA (Damn Vulnerable Web Application)
- MariaDB Database

Apache serves the DVWA application on **port 8080**, while SafeLine acts as the reverse proxy and exposes the application securely over **HTTPS (port 443)**.

---

### Kali Linux VM

The Kali Linux virtual machine acts as the client and attack machine.

Tools used include:

- Firefox Browser
- curl
- ApacheBench (ab)

Kali is used to generate legitimate traffic and simulate common web attacks against the protected application.

---

# Network Topology

The virtual machines are configured using **VMware Bridged Networking**.

```
                 Physical Network
                        │
        ┌───────────────┴───────────────┐
        │                               │
   Ubuntu VM                      Kali Linux VM
(SafeLine + DVWA)                (Attack Machine)
```

Using Bridged Networking allows both virtual machines to obtain IP addresses from the same local network and communicate directly.

---

# Request Flow

The request flow through the environment is as follows:

1. The client sends an HTTPS request to `https://dvwa.local`.
2. SafeLine WAF receives the request on **port 443**.
3. SafeLine decrypts the HTTPS traffic and inspects the request.
4. SafeLine evaluates the request against its configured security policies.
5. If the request is malicious, SafeLine blocks it and records the event.
6. If the request is legitimate, SafeLine forwards it to Apache on **port 8080**.
7. Apache processes the request and serves the DVWA application.
8. DVWA interacts with the MariaDB database when required.
9. The response is returned to Apache.
10. SafeLine forwards the response back to the client over HTTPS.

---

# Port Configuration

| Port | Service | Purpose |
|------|---------|---------|
| 443 | SafeLine WAF | HTTPS Reverse Proxy |
| 8080 | Apache | Backend Web Server |
| 3306 | MariaDB | Database |
| 9443 | SafeLine Dashboard | Management Console |

---

# Components

## SafeLine WAF

Responsibilities:

- HTTPS Termination
- Reverse Proxy
- Request Inspection
- SQL Injection Detection
- XSS Detection
- HTTP Flood Defense
- Authentication Gateway
- Custom Deny Rules
- Attack Logging
- Security Monitoring

---

## Apache Web Server

Responsibilities:

- Hosts the DVWA application
- Processes requests forwarded by SafeLine
- Communicates with MariaDB

---

## DVWA

DVWA (Damn Vulnerable Web Application) is an intentionally vulnerable web application used for learning and demonstrating web application security concepts.

In this lab, DVWA acts as the protected backend application.

---

## MariaDB

MariaDB stores the DVWA database, including:

- User accounts
- Vulnerability data
- Application records

---

# Security Features Implemented

The following SafeLine security features were configured and tested in this lab:

- SQL Injection Protection
- Cross-Site Scripting (XSS) Protection
- HTTP Flood Defense
- Authentication Gateway
- Custom URL Blocking
- Custom Request Header Blocking
- HTTPS Reverse Proxy
- Attack Logging

---

# Attack Flow

```
Kali Linux
     │
     │ HTTPS Request
     ▼
SafeLine WAF
     │
     ├── Inspect Request
     │
     ├── Malicious?
     │      │
     │      ├── Yes → Block + Log Attack
     │      │
     │      └── No
     ▼
Apache Web Server
     ▼
DVWA
     ▼
MariaDB
```

---

# Summary

This architecture demonstrates how a Web Application Firewall can be deployed in front of a vulnerable web application to inspect incoming requests, enforce security policies, block malicious traffic, and log security events while allowing legitimate users to access the application securely over HTTPS.
