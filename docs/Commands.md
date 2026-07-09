# Commands Reference

This document contains commonly used commands for managing and verifying the SafeLine WAF + DVWA lab.

---

# System Information

Ubuntu Version

```bash
lsb_release -a
```

Apache Version

```bash
apache2 -v
```

PHP Version

```bash
php -v
```

Docker Version

```bash
docker --version
```

MariaDB Version

```bash
mariadb --version
```

---

# Service Management

Apache

```bash
sudo systemctl start apache2
sudo systemctl stop apache2
sudo systemctl restart apache2
sudo systemctl status apache2
```

MariaDB

```bash
sudo systemctl start mariadb
sudo systemctl restart mariadb
sudo systemctl status mariadb
```

Docker

```bash
sudo systemctl start docker
sudo systemctl restart docker
sudo systemctl status docker
```

---

# Network Verification

Apache Port

```bash
sudo ss -tlnp | grep 8080
```

HTTPS Port

```bash
sudo ss -tlnp | grep :443
```

Ubuntu IP

```bash
hostname -I
```

---

# Docker

Running Containers

```bash
docker ps
```

All Containers

```bash
docker ps -a
```

Container Logs

```bash
docker logs <container-name>
```

---

# Apache

Restart

```bash
sudo systemctl restart apache2
```

Test Backend

```bash
curl http://127.0.0.1:8080/DVWA/
```

---

# SafeLine Verification

```bash
curl -k -I https://dvwa.local
```

---

# HTTP Flood Test

Install ApacheBench

```bash
sudo apt install apache2-utils
```

Generate Requests

```bash
ab -n 200 -c 50 https://dvwa.local/DVWA/
```

---

# Custom Rule Test

```bash
curl -k https://dvwa.local
```

---

# Hosts File

```bash
cat /etc/hosts
```

Edit

```bash
sudo nano /etc/hosts
```
