# 🛡️ SafeLine WAF + DVWA Security Lab

![Platform](https://img.shields.io/badge/Platform-Ubuntu%2026.04-orange)
![SafeLine](https://img.shields.io/badge/SafeLine-9.3.9-blue)
![Apache](https://img.shields.io/badge/Apache-2.4.66-red)
![PHP](https://img.shields.io/badge/PHP-8.5.4-777BB4)
![MariaDB](https://img.shields.io/badge/MariaDB-11.8.6-blue)
![Docker](https://img.shields.io/badge/Docker-29.6.1-2496ED)
![DVWA](https://img.shields.io/badge/DVWA-Latest-green)

## 📌 Project Overview

This project demonstrates the deployment of a Web Application Firewall (WAF) using **SafeLine** to protect the **Damn Vulnerable Web Application (DVWA)** running on an Apache web server.

The objective of this lab is to understand how a modern WAF can inspect HTTP/HTTPS traffic, detect malicious requests, enforce security policies, and protect vulnerable web applications from common attacks.

The environment was built using Ubuntu, Docker, Apache, MariaDB, PHP, SafeLine WAF, and Kali Linux for security testing.

---

## 🎯 Objectives

- Deploy DVWA on Ubuntu
- Configure Apache as the backend web server
- Deploy SafeLine WAF using Docker
- Configure Reverse Proxy
- Enable HTTPS using SSL certificates
- Protect DVWA using SafeLine
- Demonstrate attack detection and prevention
- Understand WAF configuration and rule management

---

## 🚀 Features

- Reverse Proxy Configuration
- HTTPS Protection
- SSL Certificate Configuration
- SQL Injection Detection & Blocking
- Cross-Site Scripting (XSS) Protection
- HTTP Flood Defense (Rate Limiting)
- Authentication Gateway
- Custom URL Blocking Rules
- Custom Request Header Blocking Rules
- Attack Logging & Monitoring

---

## 🛠️ Technologies Used

| Component | Version |
|-----------|---------|
| Ubuntu | 26.04 LTS |
| Apache | 2.4.66 |
| PHP | 8.5.4 |
| MariaDB | 11.8.6 |
| SafeLine WAF | 9.3.9 |
| Docker | 29.6.1 |
| Kali Linux | 2026.3 |
| DVWA | Latest GitHub Version |

---

## 🏗️ Lab Architecture

![Architecture](images/architecture.png)

```
Kali Linux
      │
 HTTPS Requests
      │
      ▼
+---------------------+
|    SafeLine WAF     |
|---------------------|
| Reverse Proxy       |
| SQL Injection       |
| XSS Protection      |
| HTTP Flood Defense  |
| Authentication      |
| Custom Rules        |
| Attack Logging      |
+---------------------+
      │
 HTTP (8080)
      │
      ▼
+---------------------+
| Apache Web Server   |
+---------------------+
      │
      ▼
+---------------------+
|        DVWA         |
+---------------------+
      │
      ▼
+---------------------+
|      MariaDB        |
+---------------------+
```

---

## 📂 Repository Structure

```text
SafeLine-DVWA-WAF-Lab/
├── README.md
├── docs/
├── images/
├── scripts/
└── apache/
```

---

## 📸 Screenshots

The repository includes screenshots demonstrating:

- SafeLine Dashboard
- Reverse Proxy Configuration
- DVWA Login
- SQL Injection Protection
- XSS Protection
- HTTP Flood Defense
- Authentication Gateway
- Custom Deny Rules
- Attack Logs
