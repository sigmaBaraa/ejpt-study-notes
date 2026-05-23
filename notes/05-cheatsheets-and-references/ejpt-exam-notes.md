# ejpt exam notes

Completed in INE: No

## 192.168.100.0/24 (External)

| Target | OS | Services / Ports | Creds | Vuln / Vector | Notes |
| --- | --- | --- | --- | --- | --- |
| 192.168.100.50
wordpress.local
WINSERVER-01 | Windows Server 2012 R2 | 80 → Apache/PHP (Apache 2.4.51, PHP 7.4.26)
445 → SMB
3307 → DB service
3389 → RDP
5985 / 47001 → WinRM / HTTPAPI | admin / estrella
mike / diamond | WordPress 5.9.3 | عليه خدمة قاعدة بيانات على 3307 |
| 192.168.100.51
WINSERVER-02 | Windows Server 2012 family | 21 → FTP
80 → IIS 8.5
445 → SMB
3389 → RDP
5985 / 47001 → WinRM / HTTPAPI | - | Command injection | Anonymous FTP enabled
ملفات في FTP: `cmdasp`.aspx, robots.txt.tx
WebDAV enabled |
| 192.168.100.52 | Ubuntu Linux | 21 → FTP
22 → SSH
25 → SMTP (OpenSMTPD)
80 → Apache
139/445 → Samba
3306 → MySQL/MariaDB
3389 → xrdp | DB (drupal):
database: drupal
username: drupal
password: syntex0421
host: [localhost](http://localhost)
port: 3306 | - | Drupal username = system password
Anonymous FTP enabled
`updates`.txt
/drupal 7 |
| 192.168.100.55
WINSERVER-03 | Windows Server 2019 Datacenter | 80 → IIS 10.0
445 → SMB
3389 → RDP
5985 / 47001 → WinRM / HTTPAPI | Administrator / swordfish | - | Internal network pivot (access via psexec model) |
| 192.168.100.63 | Windows | 3389 → RDP
5985 → WinRM | - | - | - |
| 192.168.100.67 | Ubuntu Linux | 22 → SSH فقط | - | - | - |
| 192.168.100.5 | - | - | - | - | - |

---

## Notes: Drupal (DB dump)

- MariaDB (drupal) → `SELECT * FROM users;` (output preserved below)

drupal  info:

ariaDB [drupal]> SELECT * FROM users;
+-----+----------+---------------------------------------------------------+----------------------+-------+-----------+------------------+------------+------------+------------+--------+------------------+----------+---------+----------------------+------+
| uid | name     | pass                                                    | mail                 | theme | signature | signature_format | created    | access     | login      | status | timezone         | language | picture | init                 | data |
+-----+----------+---------------------------------------------------------+----------------------+-------+-----------+------------------+------------+------------+------------+--------+------------------+----------+---------+----------------------+------+
|   0 |          |                                                         |                      |       |           | NULL             |          0 |          0 |          0 |      0 | NULL             |          |       0 |                      | NULL |
|   1 | admin    | $S$D67i0qFmSLMLwZ9PU7VEocSS9fvV1JaSeJxQMgCid80hGbq6wXZH | [admin@syntex.com](mailto:admin@syntex.com)     |       |           | NULL             | 1650232322 | 1650248652 | 1650248498 |      1 | America/New_York |          |       0 | [admin@syntex.com](mailto:admin@syntex.com)     | b:0; |
|   2 | auditor  | $S$DV.wsqkmKY3y5VW.icW/g5NTU3h.UA01nxqL9Cro27GaSBYpH4WC | [auditor@syntex.com](mailto:auditor@syntex.com)   |       |           | filtered_html    | 1650234408 |          0 |          0 |      1 | America/New_York |          |       0 | [auditor@syntex.com](mailto:auditor@syntex.com)   | b:0; |
|   3 | dbadmin  | $S$DZcGD5qcb6xso1E/Mu6DJP4uPi5DfY28kBEyuIab8Pod1saBaImN | [dbadmin@syntex.com](mailto:dbadmin@syntex.com)   |       |           | filtered_html    | 1650248436 |          0 |          0 |      1 | America/New_York |          |       0 | [dbadmin@syntex.com](mailto:dbadmin@syntex.com)   | b:0; |
|   4 | Vincenzo | $S$DGnS.dK3q2FeWeNbLikdI5Hk/XdBFI2jBFkmPvv/v9Ln8vjIanIu | [vincenzo@syntext.com](mailto:vincenzo@syntext.com) |       |           | filtered_html    | 1650248490 |          0 |          0 |      1 | America/New_York |          |       0 | [vincenzo@syntext.com](mailto:vincenzo@syntext.com) | b:0; |
+-----+----------+---------------------------------------------------------+----------------------+-------+-----------+------------------+------------+------------+------------+--------+------------------+----------+---------+----------------------+------+

---

## WINSERVER-03: Hashes

| User | NTLM Hash |
| --- | --- |
| Administrator | `61fb34469b9989b01be4e8630c52eed6` |
| student | `bd4ca1fbe028f3c5066467a7f6a73b0b` |
| lawrence | `18aa104784f77431563b1a1b67f6096c` |
| mary | `11637a16fca11b3604e3e68d5221b3c7` |
| admin | `0f2011271b98907e6d288066567d3319` |

Credentials (from cracking):

- lawrence → computadora
- mary → hotmama
- admin → blanca

---

## Internal network: 192.168.0.0/24

Discovered hosts:

- 192.168.0.1
- 192.168.0.50
- 192.168.0.51
- 192.168.0.57

| IP | OS | Vuln / Notes | Services | Info |
| --- | --- | --- | --- | --- |
| 192.168.0.50
WINSERVER-03 | Windows Server 2019 | - | 80 → IIS 10.0
139/445 → SMB
3389 → RDP
5985 → WinRM | - |
| 192.168.0.51 | Ubuntu Linux | Webmin through msf | 22 → OpenSSH 8.2p1
80 → Apache 2.4.41
3389 → RDP
10000 → miniServ 1.920 | http-title: Login to Webmin
robots.txt: 1 disallowed entry |
| 192.168.0.57 | Ubuntu Linux | - | 22 → SSH | - |
| 192.168.0.1 | - | - | - | - |