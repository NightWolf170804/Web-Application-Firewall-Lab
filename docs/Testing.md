# Security Testing

This document describes the attack scenarios used to validate the SafeLine WAF deployment.

---

# SQL Injection

## Payload

```sql
1' OR '1'='1
```

Expected

- Access Forbidden
- SQL Injection detected
- Attack logged

---

# Cross Site Scripting (XSS)

Payload

```html
<script>alert('XSS')</script>
```

Expected

- Access Forbidden
- XSS detected
- Attack logged

---

# HTTP Flood Defense

Command

```bash
ab -n 200 -c 50 https://dvwa.local/DVWA/
```

Expected

- Rate limiting
- HTTP Flood event generated

---

# Authentication Gateway

Expected

- Login page displayed before DVWA

---

# URL Blocking

Blocked URL

```
/DVWA/vulnerabilities/brute/
```

Expected

- Access Forbidden

---

# Request Header Blocking

Rule

```
Req Headers

Contains

curl
```

Command

```bash
curl -k https://dvwa.local
```

Expected

- Request denied

---

# Attack Logs

Verify

- SQL Injection
- XSS
- HTTP Flood
- Custom Deny Rules

are visible in

```
SafeLine

↓

Attacks

↓

Logs
```
