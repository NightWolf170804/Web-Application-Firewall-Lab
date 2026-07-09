# Installation Guide

This guide explains how to deploy the SafeLine Web Application Firewall (WAF) and the Damn Vulnerable Web Application (DVWA) in a virtual lab environment.

---

# Lab Environment

| Component | Version |
|-----------|---------|
| Operating System | Ubuntu 26.04 LTS |
| Web Server | Apache 2.4.66 |
| PHP | 8.5.4 |
| Database | MariaDB 11.8.6 |
| Docker | 29.6.1 |
| SafeLine WAF | 9.3.9 |
| Attacker Machine | Kali Linux 2026.3 |
| Vulnerable Application | DVWA |
| Virtualization | VMware Workstation |

---

# Network Configuration

The lab uses **VMware Bridged Networking**, allowing both virtual machines to communicate directly on the same network.

## Ubuntu VM

- SafeLine WAF
- Apache Web Server
- PHP
- MariaDB
- DVWA

## Kali Linux VM

- Firefox
- curl
- ApacheBench (ab)

---

# Lab Architecture

```
                Kali Linux
                     │
              HTTPS (443)
                     │
              SafeLine WAF
                     │
           Reverse Proxy
                     │
             Apache (8080)
                     │
                  DVWA
                     │
                 MariaDB
```

---

# Step 1 - Prepare Ubuntu

Update Ubuntu.

```bash
sudo apt update
sudo apt upgrade -y
```

Install required packages.

```bash
sudo apt install apache2 mariadb-server php php-mysqli php-gd php-xml php-curl php-mbstring git docker.io -y
```

Verify the installation.

```bash
apache2 -v
php -v
mariadb --version
docker --version
```

---

# Step 2 - Enable Required Services

Enable services at boot.

```bash
sudo systemctl enable apache2
sudo systemctl enable mariadb
sudo systemctl enable docker
```

Start services.

```bash
sudo systemctl start apache2
sudo systemctl start mariadb
sudo systemctl start docker
```

Verify the services.

```bash
sudo systemctl status apache2
sudo systemctl status mariadb
docker ps
```

---

# Step 3 - Configure Apache

SafeLine listens on HTTPS (Port 443). Apache was configured to listen on **Port 8080**.

Edit:

```text
/etc/apache2/ports.conf
```

Change

```text
Listen 80
```

to

```text
Listen 8080
```

Next edit

```text
/etc/apache2/sites-available/000-default.conf
```

Change

```text
<VirtualHost *:80>
```

to

```text
<VirtualHost *:8080>
```

Restart Apache.

```bash
sudo systemctl restart apache2
```

Verify.

```bash
sudo ss -tlnp | grep 8080
```

---

# Step 4 - Deploy DVWA

Navigate to Apache's document root.

```bash
cd /var/www/html
```

Clone DVWA.

```bash
sudo git clone https://github.com/digininja/DVWA.git
```

Create the configuration file.

```bash
cd DVWA/config

sudo cp config.inc.php.dist config.inc.php
```

---

# Step 5 - Configure MariaDB

Open MariaDB.

```bash
sudo mysql
```

Create the database.

```sql
CREATE DATABASE dvwa;

CREATE USER 'dvwa'@'localhost' IDENTIFIED BY 'password';

GRANT ALL PRIVILEGES ON dvwa.* TO 'dvwa'@'localhost';

FLUSH PRIVILEGES;

EXIT;
```

---

# Step 6 - Configure DVWA

Edit

```text
/var/www/html/DVWA/config/config.inc.php
```

Configure the database settings.

```text
Database Server : 127.0.0.1
Database Name   : dvwa
Username        : dvwa
Password        : password
```

Set file permissions.

```bash
sudo chown -R www-data:www-data /var/www/html/DVWA

sudo chmod -R 755 /var/www/html/DVWA
```

Restart Apache.

```bash
sudo systemctl restart apache2
```

---

# Step 7 - Install SafeLine WAF

Install SafeLine using the official installation instructions.

After installation verify that all containers are running.

```bash
docker ps
```

Access the SafeLine dashboard.

```text
https://<Ubuntu-IP>:9443
```

---

# Step 8 - Configure Reverse Proxy

Create a new application in SafeLine.

Use the following settings.

| Setting | Value |
|----------|-------|
| Application Name | DVWA |
| Domain | dvwa.local |
| Protocol | HTTPS |
| Backend | http://127.0.0.1:8080 |

Enable the application after saving the configuration.

---

# Step 9 - Configure HTTPS

Associate an SSL certificate with the DVWA application inside SafeLine.

All client traffic is terminated by SafeLine over HTTPS before being forwarded to Apache.

Verify HTTPS is available.

```bash
sudo ss -tlnp | grep :443
```

---

# Step 10 - Configure Local Hostname

On the Kali Linux VM, edit the hosts file.

```bash
sudo nano /etc/hosts
```

Add the Ubuntu VM IP address.

```text
192.168.xxx.xxx    dvwa.local
```

Replace the IP address with the actual address assigned to the Ubuntu virtual machine.

Verify name resolution.

```bash
ping dvwa.local
```

---

# Step 11 - Initialize DVWA

Open

```text
https://dvwa.local/DVWA/setup.php
```

Click

```text
Create / Reset Database
```

Log in using the default credentials.

```text
Username : admin

Password : password
```

---

# Step 12 - Verify the Deployment

Ensure the following checks are successful.

- Apache service is running.
- MariaDB service is running.
- Docker containers are running.
- SafeLine dashboard is accessible.
- Apache listens on port 8080.
- SafeLine listens on port 443.
- DVWA loads successfully through `https://dvwa.local/DVWA/`.
- Request statistics increase in the SafeLine dashboard when accessing the application.

---

# Next Steps

Once the deployment is complete, configure and validate the security features documented in:

- `docs/Testing.md`
- `docs/Commands.md`
- `docs/Troubleshooting.md`
- `docs/Architecture.md`
