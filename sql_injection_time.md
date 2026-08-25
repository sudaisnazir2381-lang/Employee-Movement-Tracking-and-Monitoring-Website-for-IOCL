# Title: [sql-injection] in [Employee-Movement-Tracking-and-Monitoring-Website-for-IOCL] <= [v1.0]

**BUG Author:** Sudais Nazir

---

# Product Information

- **Software Link:** https://github.com/bhagya3929/Employee-Movement-Tracking-and-Monitoring-Website-for-IOCL
- **Affected Version:** <= v1.0
- **BUG Author:** Sudais Nazir

---

# Vulnerability Details

| Field | Value |
|---------|---------|
| **Type** | Time Based SQL Injection |
| **Affected URL** | `http://localhost/Employee-Movement-Tracking-and-Monitoring-Website-for-IOCL/admin_transaction.php` |
| **Vulnerable Parameter** | `username` |

## Vulnerable Files

| File Name | Path |
|------------|------|
| `admin_transaction.php` | `/Employee-Movement-Tracking-and-Monitoring-Website-for-IOCL/admin_transaction.php` |


---

# Vulnerability Type

- **CWE:** CWE-89 (SQL Injection)
- **Severity:** CRITICAL
- **CVSS Score:** 9.1

---

# Root Cause

The code directly inserts user-provided data from `$_POST` (e.g., `spassword='$oldpass'`) into SQL queries without sanitization, allowing attackers to manipulate queries.

## Screenshot

<img width="833" height="116" alt="capture1" src="https://github.com/user-attachments/assets/bd06e702-964c-4461-8cbb-17b90872d5d4" />

# Impact

- Unauthorized access to database information.
- Potential exposure of sensitive information (such as user passwords).
- Possible database corruption or data manipulation.

---

# Description

## 1. Vulnerability Details

- In this PHP code, the `username` parameter is directly concatenated into SQL Statement.
- No input validation or escaping mechanisms implemented.

## 2. Attack Vectors

- Attackers can manipulate SQL query structure using special characters.
- Additional information can be extracted using Time Based Payloads.
- Database information can be obtained through Time Based injection.
- Time based injection might reveal more information.

## 3. Attack Payload Examples

### SQL Payload

```sql
' AND (SELECT 3965 FROM (SELECT(SLEEP(5)))yPgs) AND 'Eryu'='Eryu

```

`username` is injectable.
<img width="1079" height="78" alt="proof" src="https://github.com/user-attachments/assets/056e86d0-96d8-40ac-9ef1-480b7deea90b" />


# Proof of Concept

## Information Extraction

### Payload

```sql
' AND (SELECT 2713 FROM (SELECT(SLEEP(5)))pQmt) AND 'cuFH'='cuFH
```

### Result

`username` is injectable.

### Screenshot

<img width="252" height="56" alt="proof" src="https://github.com/user-attachments/assets/4da75e16-8c9f-4a5a-9267-a96abdee8169" />




