# CVE-2026-105776: Time-Based SQL Injection in Employee-Movement-Tracking-and-Monitoring-Website-for-IOCL 1.0

| | |
|---|---|
| **CVE** | CVE-2026-105776 ([NVD](https://nvd.nist.gov/vuln/detail/CVE-2026-105776) · [VulDB](https://vuldb.com/cve/CVE-2026-105776)) |
| **Discovered by** | Sudais Nazir |
| **Published** | October 2026 |
| **Product** | [Employee-Movement-Tracking-and-Monitoring-Website-for-IOCL](https://github.com/bhagya3929/Employee-Movement-Tracking-and-Monitoring-Website-for-IOCL) (open-source PHP application) |
| **Affected version** | <= 1.0 |
| **Weakness** | CWE-89: SQL Injection |
| **Severity** | High, CVSS 3.1 base score 7.3 |
| **CVSS vector** | **[CHECK: copy the exact vector from the NVD/VulDB record]** |

---

## Summary

The `username` parameter in `admin_transaction.php` is concatenated directly into an SQL query. An attacker can inject SQL and use time-based techniques to extract data from the database, including database and table names. No sanitization or prepared statements are used.

## Vulnerability Details

| Field | Value |
|---|---|
| **Type** | Time-based blind SQL injection |
| **Affected file** | `admin_transaction.php` |
| **Affected URL** | `http://localhost/Employee-Movement-Tracking-and-Monitoring-Website-for-IOCL/admin_transaction.php` |
| **Vulnerable parameter** | `username` (HTTP **[FILL: POST/GET]**) |
| **Authentication required** | **[FILL: Yes, which role / No]** |

## Root Cause

The value of `username` is placed straight into the query string, so anything the user types becomes part of the SQL statement.

**[FILL: replace this block with the exact vulnerable lines from your screenshot]**

```php
$username = $_POST['username'];
$query = "SELECT ... FROM ... WHERE username='$username' ...";
```

The screenshot below shows the vulnerable code:

![Root cause: vulnerable code](https://github.com/user-attachments/assets/bd06e702-964c-4461-8cbb-17b90872d5d4)

## Proof of Concept

**Test environment:** PHP **[FILL version]**, MySQL/MariaDB **[FILL version]**, local XAMPP/LAMP install of the application, version 1.0.

### Steps to reproduce

1. Install the application locally **[FILL: and log in as ..., or note that no login is needed]**.
2. Open `/admin_transaction.php`.
3. Intercept the request with Burp Suite and send the following value in the `username` parameter:

   ```
   ' AND (SELECT 3965 FROM (SELECT(SLEEP(5)))yPgs) AND 'Eryu'='Eryu
   ```

4. Compare response times. A normal request returns in about **[FILL: e.g. 0.1] seconds**; the injected request is delayed by about **5 seconds**, which confirms the injected `SLEEP(5)` executed on the database.

**[FILL: paste the full raw HTTP request from Burp here, with cookies redacted]**

Request showing the payload in `username`:

![Payload in the username parameter](https://github.com/user-attachments/assets/056e86d0-96d8-40ac-9ef1-480b7deea90b)

Confirmation that `username` is injectable:

![Injection confirmed](https://github.com/user-attachments/assets/0c71e2f0-a1ca-4374-bb6c-4b7d153aecea)

### Data extraction

Using the same injection point, I extracted the database names and then the table names, showing that arbitrary data in the database can be read.

Database enumeration:

![Database names extracted](https://github.com/user-attachments/assets/4da75e16-8c9f-4a5a-9267-a96abdee8169)

Table enumeration:

![Table names extracted](https://github.com/user-attachments/assets/4b98fb43-05e2-46f6-b1a9-38cc9b0088c4)

## Impact

Through this injection point, a user can read data from the application's database, which may include employee records and stored credentials. Impact is limited to what the database account can access. Reading data was demonstrated; modification of data was not tested.

## Remediation

Use prepared statements so user input is never treated as SQL (adjust the query to match the real one):

```php
$stmt = $conn->prepare("SELECT * FROM users WHERE username = ?");
$stmt->bind_param("s", $username);
$stmt->execute();
$result = $stmt->get_result();
```

Additional hardening:

- Run the application with a database account that has only the privileges it needs.
- Validate the type and length of all submitted fields.
- Store passwords with `password_hash()` and check them with `password_verify()`.

## Evidence URLs

1. Root cause (vulnerable code): https://github.com/user-attachments/assets/bd06e702-964c-4461-8cbb-17b90872d5d4
2. Payload in the `username` parameter: https://github.com/user-attachments/assets/056e86d0-96d8-40ac-9ef1-480b7deea90b
3. Injection confirmed: https://github.com/user-attachments/assets/0c71e2f0-a1ca-4374-bb6c-4b7d153aecea
4. Database names extracted: https://github.com/user-attachments/assets/4da75e16-8c9f-4a5a-9267-a96abdee8169
5. Table names extracted: https://github.com/user-attachments/assets/4b98fb43-05e2-46f6-b1a9-38cc9b0088c4

---

## Disclosure Timeline

| Date | Event |
|---|---|
| **[FILL: date]** | Reported to **[FILL: VulDB / maintainer]** |
| October 2026 | CVE-2026-105776 published |

## References

- [NVD: CVE-2026-105776](https://nvd.nist.gov/vuln/detail/CVE-2026-105776)
- [VulDB: CVE-2026-105776](https://vuldb.com/cve/CVE-2026-105776)
- [OWASP SQL Injection Prevention Cheat Sheet](https://cheatsheetseries.owasp.org/cheatsheets/SQL_Injection_Prevention_Cheat_Sheet.html)
- [CWE-89: SQL Injection](https://cwe.mitre.org/data/definitions/89.html)
- [PHP Manual: Prepared statements](https://www.php.net/manual/en/mysqli.quickstart.prepared-statements.php)
