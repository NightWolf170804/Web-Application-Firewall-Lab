# Troubleshooting

This document records issues encountered while building the lab and the solutions used.

---

# Apache Redirected to Port 8080

Problem

```
https://dvwa.local

↓

http://dvwa.local:8080
```

Cause

Browser cached an old redirect.

Solution

- Clear browser cache.
- Use a Private/Incognito window.
- Access

```
https://dvwa.local/DVWA/
```

---

# SQL Injection Not Detected

Cause

Requests bypassed SafeLine.

Solution

Always use

```
https://dvwa.local
```

instead of

```
http://<Ubuntu-IP>:8080
```

---

# HTTP Flood Not Triggering

Cause

Threshold configured too high.

Solution

Reduce the request threshold.

---

# curl Rule Not Working

Cause

Matching the full User-Agent string failed.

Solution

Configure:

Match Target

```
Req Headers
```

Operator

```
Contains
```

Content

```
curl
```

---

# dvwa.local Not Resolving

Cause

Missing hosts file entry.

Solution

Edit

```bash
sudo nano /etc/hosts
```

Add

```
192.168.xxx.xxx dvwa.local
```

---

# HTTPS Certificate Warning

Cause

Self-signed certificate.

Solution

Proceed through the browser warning in the lab environment or trust the certificate if desired.

---

# SafeLine Dashboard Not Opening

Verify

```bash
docker ps
```

Restart Docker

```bash
sudo systemctl restart docker
```

---

# Apache Not Running

Check

```bash
sudo systemctl status apache2
```

Restart

```bash
sudo systemctl restart apache2
```
