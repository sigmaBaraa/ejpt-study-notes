# eJPT Exam Notes

These are my personal notes taken during the eJPT exam workflow. They include discovered hosts, services, credentials, vulnerabilities, hashes, pivoting information, and internal-network findings.

Original progress marker: `Completed in INE: No`

## External Network: 192.168.100.0/24

### 192.168.100.50 - wordpress.local / WINSERVER-01

**Operating System:** Windows Server 2012 R2

**Services and Ports:**

- `80` → Apache/PHP (`Apache 2.4.51`, `PHP 7.4.26`)
- `445` → SMB
- `3307` → DB service
- `3389` → RDP
- `5985 / 47001` → WinRM / HTTPAPI

**Credentials:**

- `admin / estrella`
- `mike / diamond`

**Vulnerability / Vector:**

- WordPress `5.9.3`

**Notes:**

- يوجد عليه خدمة قاعدة بيانات على `3307`

---

### 192.168.100.51 - WINSERVER-02

**Operating System:** Windows Server 2012 family

**Services and Ports:**

- `21` → FTP
- `80` → IIS `8.5`
- `445` → SMB
- `3389` → RDP
- `5985 / 47001` → WinRM / HTTPAPI

**Credentials:**

- None

**Vulnerability / Vector:**

- Command injection

**Notes:**

- Anonymous FTP enabled
- ملفات في FTP:
  - `cmdasp.aspx`
  - `robots.txt.tx`
- WebDAV enabled

---

### 192.168.100.52

**Operating System:** Ubuntu Linux

**Services and Ports:**

- `21` → FTP
- `22` → SSH
- `25` → SMTP (`OpenSMTPD`)
- `80` → Apache
- `139/445` → Samba
- `3306` → MySQL/MariaDB
- `3389` → xrdp

**Database Credentials - Drupal:**

- Database: `drupal`
- Username: `drupal`
- Password: `syntex0421`
- Host: [localhost](http://localhost)
- Port: `3306`

**Vulnerability / Vector:**

- Not specified

**Notes:**

- Drupal username = system password
- Anonymous FTP enabled
- `updates.txt`
- `/drupal 7`

---

### 192.168.100.55 - WINSERVER-03

**Operating System:** Windows Server 2019 Datacenter

**Services and Ports:**

- `80` → IIS `10.0`
- `445` → SMB
- `3389` → RDP
- `5985 / 47001` → WinRM / HTTPAPI

**Credentials:**

- `Administrator / swordfish`

**Vulnerability / Vector:**

- Not specified

**Notes:**

- Internal network pivot
- Access via PsExec model

---

### 192.168.100.63

**Operating System:** Windows

**Services and Ports:**

- `3389` → RDP
- `5985` → WinRM

**Credentials:** None

**Vulnerability / Vector:** Not specified

**Notes:** None

---

### 192.168.100.67

**Operating System:** Ubuntu Linux

**Services and Ports:**

- `22` → SSH فقط

**Credentials:** None

**Vulnerability / Vector:** Not specified

**Notes:** None

---

### 192.168.100.5

**Operating System:** Not specified

**Services and Ports:** Not specified

**Credentials:** None

**Vulnerability / Vector:** Not specified

**Notes:** None

---

## Drupal Notes - Database Dump

MariaDB database: `drupal`

Command used:

```sql
SELECT * FROM users;
```

Output preserved below:

```text
MariaDB [drupal]> SELECT * FROM users;
+-----+----------+---------------------------------------------------------+----------------------+-------+-----------+------------------+------------+------------+------------+--------+------------------+----------+---------+----------------------+------+
| uid | name     | pass                                                    | mail                 | theme | signature | signature_format | created    | access     | login      | status | timezone         | language | picture | init                 | data |
+-----+----------+---------------------------------------------------------+----------------------+-------+-----------+------------------+------------+------------+------------+--------+------------------+----------+---------+----------------------+------+
|   0 |          |                                                         |                      |       |           | NULL             |          0 |          0 |          0 |      0 | NULL             |          |       0 |                      | NULL |
|   1 | admin    | $S$D67i0qFmSLMLwZ9PU7VEocSS9fvV1JaSeJxQMgCid80hGbq6wXZH | admin@syntex.com     |       |           | NULL             | 1650232322 | 1650248652 | 1650248498 |      1 | America/New_York |          |       0 | admin@syntex.com     | b:0; |
|   2 | auditor  | $S$DV.wsqkmKY3y5VW.icW/g5NTU3h.UA01nxqL9Cro27GaSBYpH4WC | auditor@syntex.com   |       |           | filtered_html    | 1650234408 |          0 |          0 |      1 | America/New_York |          |       0 | auditor@syntex.com   | b:0; |
|   3 | dbadmin  | $S$DZcGD5qcb6xso1E/Mu6DJP4uPi5DfY28kBEyuIab8Pod1saBaImN | dbadmin@syntex.com   |       |           | filtered_html    | 1650248436 |          0 |          0 |      1 | America/New_York |          |       0 | dbadmin@syntex.com   | b:0; |
|   4 | Vincenzo | $S$DGnS.dK3q2FeWeNbLikdI5Hk/XdBFI2jBFkmPvv/v9Ln8vjIanIu | vincenzo@syntext.com |       |           | filtered_html    | 1650248490 |          0 |          0 |      1 | America/New_York |          |       0 | vincenzo@syntext.com | b:0; |
+-----+----------+---------------------------------------------------------+----------------------+-------+-----------+------------------+------------+------------+------------+--------+------------------+----------+---------+----------------------+------+
```

---

## WINSERVER-03 Hashes

| User | NTLM Hash |
| --- | --- |
| Administrator | `61fb34469b9989b01be4e8630c52eed6` |
| student | `bd4ca1fbe028f3c5066467a7f6a73b0b` |
| lawrence | `18aa104784f77431563b1a1b67f6096c` |
| mary | `11637a16fca11b3604e3e68d5221b3c7` |
| admin | `0f2011271b98907e6d288066567d3319` |

### Cracked Credentials

- `lawrence` → `computadora`
- `mary` → `hotmama`
- `admin` → `blanca`

---

## Internal Network: 192.168.0.0/24

### Discovered Hosts

- `192.168.0.1`
- `192.168.0.50`
- `192.168.0.51`
- `192.168.0.57`

---

### 192.168.0.50 - WINSERVER-03

**Operating System:** Windows Server 2019

**Services and Ports:**

- `80` → IIS `10.0`
- `139/445` → SMB
- `3389` → RDP
- `5985` → WinRM

**Vulnerability / Notes:**

- Not specified

**Info:**

- Not specified

---

### 192.168.0.51

**Operating System:** Ubuntu Linux

**Services and Ports:**

- `22` → OpenSSH `8.2p1`
- `80` → Apache `2.4.41`
- `3389` → RDP
- `10000` → miniServ `1.920`

**Vulnerability / Notes:**

- Webmin through MSF

**Info:**

- `http-title: Login to Webmin`
- `robots.txt: 1 disallowed entry`

---

### 192.168.0.57

**Operating System:** Ubuntu Linux

**Services and Ports:**

- `22` → SSH

**Vulnerability / Notes:**

- Not specified

**Info:**

- Not specified

---

### 192.168.0.1

**Operating System:** Not specified

**Services and Ports:** Not specified

**Vulnerability / Notes:** Not specified

**Info:** Not specified

