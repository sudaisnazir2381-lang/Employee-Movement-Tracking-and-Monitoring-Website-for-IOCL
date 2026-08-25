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

