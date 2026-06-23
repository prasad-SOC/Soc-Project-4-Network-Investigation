# IOC List

## Investigation Details

| Field              | Value                 |
| ------------------ | --------------------- |
| Investigation Name | Network Investigation |
| Alert ID           | SOC-2026-104          |
| Severity           | Medium                |
| Status             | Closed                |

---

# Internal Asset

| IOC Type            | Value        |
| ------------------- | ------------ |
| Hostname            | WS-023       |
| Internal IP Address | 192.168.1.23 |

---

# External Indicators

## Suspicious Domain

| IOC Type | Value                |
| -------- | -------------------- |
| Domain   | cdn-update-check.com |

### Description

Domain observed during DNS analysis and HTTP communications.

---

## Suspicious IP Address

| IOC Type   | Value        |
| ---------- | ------------ |
| IP Address | 45.77.188.99 |

### Description

External destination associated with repeated outbound HTTP requests.

---

# Associated HTTP Activity

## Observed Request Pattern

```http
GET /checkin?id=WS023&seq=0 HTTP/1.1
Host: cdn-update-check.com
```

### Additional Observed Requests

```http
GET /checkin?id=WS023&seq=1 HTTP/1.1
Host: cdn-update-check.com
```

```http
GET /checkin?id=WS023&seq=2 HTTP/1.1
Host: cdn-update-check.com
```

---

# IOC Summary

| Indicator Type | Indicator            |
| -------------- | -------------------- |
| Internal Host  | WS-023               |
| Internal IP    | 192.168.1.23         |
| Domain         | cdn-update-check.com |
| External IP    | 45.77.188.99         |

---

# Analyst Notes

* Repeated communication was observed between WS-023 and 45.77.188.99.
* DNS requests were made for cdn-update-check.com.
* HTTP requests contained recurring check-in patterns.
* No evidence of malware download or data exfiltration was identified within the available packet capture.
* Indicators should be monitored for future activity and validated against organizational security policies.

---

# Recommended Actions

1. Monitor future communications involving 45.77.188.99.
2. Review endpoint activity on WS-023.
3. Validate whether cdn-update-check.com is authorized within the environment.
4. Search for these indicators across additional logs and security tools.
