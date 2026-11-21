# Redacted observation notes

These notes are transcribed from the original screenshots supplied for this lab. They omit browser URL tokens and `Set-Cookie` values. They are observations from one local test session, not a claim that the same policy is active now.

| Check | Visible observation |
| --- | --- |
| Ubuntu network | `enp0s8` had `192.168.56.10/24`. |
| Kali network | `eth1` had `192.168.56.11/24`. |
| Container exposure | DVWA showed `127.0.0.1:4280->80/tcp`; SafeLine and MariaDB containers were running in the same Ubuntu VM. |
| Current SafeLine application Edit view | After reopening the saved application, the user confirmed `dvwa.test` has only HTTPS 443. The screenshot shows the selected `dvwa.test` certificate and upstream `http://172.18.0.3`. An earlier Edit screenshot also showed HTTP 80, before its removal. |
| Direct backend from Kali | `curl http://192.168.56.10:4280/` failed to connect. |
| Normal DVWA result | A request with SQL Injection page user ID `1` displayed `First name: admin` and `Surname: admin`. |
| SQL injection sample | The browser displayed SafeLine's Access Forbidden page. SafeLine's Attacks log showed `Blocked`, `SQL Inj`, source `192.168.56.11`, and a DVWA SQL Injection URL. |
| Reflected XSS sample | The browser displayed SafeLine's Access Forbidden page. No corresponding XSS-category log screenshot was provided. |
| Access limit | A saved rule showed 3 requests in 10 seconds and a 60-minute block. SafeLine showed a trigger for `192.168.56.11` on `dvwa.test`; Kali later received `HTTP/1.1 429 Too Many Requests`. |
| Deny rule | SafeLine logged `Blocked`, `Deny Rule`, name `dvwa`, source `192.168.56.11`, URL `http://dvwa.test/login.php`. Kali received `HTTP/1.1 403 Forbidden`. |
| HTTPS | `curl -k -I https://dvwa.test/login.php` received `HTTP/1.1 200 OK`. The reopened application Edit view shows the current 443 listener and selected certificate. |

One terminal screenshot displayed DVWA's `security=impossible` cookie. A screenshot of the XSS page showed the sample script as literal text. These observations do not establish successful exploitation when SafeLine was disabled. The `429` and `403` responses are tied to the temporary test policies, which may need to be removed or disabled to resume normal lab access. The HTTP tests were recorded before the application was changed to HTTPS 443 only.
