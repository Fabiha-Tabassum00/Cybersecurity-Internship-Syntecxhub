# Cybersecurity Projects — Syntecxhub Internship

A collection of four Python security tools built during a one-month cybersecurity internship at **Syntecxhub**. Each project was assigned as a weekly task, and this repository contains my completed solution for one project from each week.

> ⚠️ **For educational and authorized use only.** These tools are intended for learning and for testing systems you own or have explicit written permission to test (e.g. DVWA, local lab machines). Unauthorized scanning or testing of systems you do not own is illegal.

## Projects

| Week | Project | Focus |
|------|---------|-------|
| 1 | [Port Scanner](week1-port-scanner/port_scanner.py) | Sockets, concurrency |
| 2 | [Encrypted Chat App](week2-encrypted-chat/encrypted_chat_app.py) | AES encryption, TCP sockets |
| 3 | [SQL Injection Scanner](week3-sql-injection-scanner/sql_injection_scanner.py) | Web input testing |
| 4 | [CVE / Vulnerability Scanner](week4-cve-scanner/cve_scanner.py) | Banner grabbing, CVE lookup |

---

## Week 1 — Port Scanner

**Task requirements:** Build a TCP port scanner that checks open ports on a host, learn socket programming and concurrency (threads), support scanning a single host across a range of ports, and print/log results (open, closed, timeouts) with exception handling.

**How it works:** The scanner takes a target IP and a port range (e.g. `1-1000`) and validates the range is within 1–65535. It splits the range into chunks and runs them across a `ThreadPoolExecutor` of up to 100 threads for speed. Each port is probed with a socket using `connect_ex`, and the numeric result is interpreted as open, closed (connection refused), or timeout. Every result is printed with a status marker and written to `scan_results.log` with a timestamp, and the total scan time is reported at the end.

**Run:**
```bash
python week1-port-scanner/port_scanner.py
# then enter the target IP and port range when prompted
```

---

## Week 2 — Encrypted Chat App

**Task requirements:** Build a client/server chat where messages are encrypted with AES before sending, implement TCP socket communication with symmetric encryption, handle a pre-shared key and safe IV usage, and support message logging.

**How it works:** Messages are encrypted with **AES in CBC mode** using a 32-byte pre-shared key. A fresh random IV is generated for every message and prepended to the ciphertext, then the whole thing is Base64-encoded before being sent over a TCP socket — so no two identical messages produce the same ciphertext. Plaintext input is PKCS#7-padded to the AES block size before encryption. Each message is logged with a timestamp to `client_chat.log`, and connection, disconnection, and error events are handled and recorded.

**Run:** Start the server, then run the client and type messages (type `quit` to exit). Requires `pycryptodome`:
```bash
pip install pycryptodome
python week2-encrypted-chat/encrypted_chat_app.py
```

---

## Week 3 — SQL Injection Scanner

**Task requirements:** Build a script that probes web inputs for common SQL injection patterns, sends crafted requests and detects vulnerability indicators, reports findings, respects legal/ethical rules (only test permitted targets like DVWA), and adds logging with basic concurrency and rate-limiting.

**How it works:** The scanner fetches a target page with BeautifulSoup and extracts every form, along with its action, method, and input fields (including text areas and selects). It first sends a clean baseline request, then injects a set of common SQLi payloads (boolean, UNION, time-based, and error-based) one at a time into each input. A response is flagged as vulnerable when it contains known database error strings or differs significantly in length from the baseline. Forms are tested concurrently with a thread pool, requests are rate-limited with a short delay, and all activity plus any findings are written to `sql_injection_scan.log`. It also prints an explicit authorized-use warning on every run.

**Run:** Requires `requests` and `beautifulsoup4`:
```bash
pip install requests beautifulsoup4
python week3-sql-injection-scanner/sql_injection_scanner.py <target-url>
```

---

## Week 4 — CVE / Vulnerability Scanner

**Task requirements:** Build a lightweight scanner that checks services/versions against known CVEs, perform banner grabbing and service detection, query CVE databases/APIs, produce a report with possible matches and severity notes, and cover responsible disclosure and triage basics.

**How it works:** The scanner probes a set of common ports on the target, then performs **banner grabbing** on any that are open — sending an HTTP request on 80/443 (with TLS on 443) and reading service banners on ports like SSH, FTP, SMTP, and MySQL. It parses each banner to identify the service and version, then queries the **NVD (National Vulnerability Database) API** for matching CVEs, extracting the CVE ID, description, and CVSS severity. Results are compiled into `vulnerability_report.txt`, which ends with a short responsible-disclosure and triage section reminding the reader not to exploit findings and to report them through proper channels. An optional NVD API key can be set for higher rate limits.

**Run:** Requires `requests`:
```bash
pip install requests
python week4-cve-scanner/cve_scanner.py
# then enter the target IP when prompted
```

---

## Tech

Python · sockets · threading · `pycryptodome` (AES) · `requests` · `beautifulsoup4` · NVD API

## Author

**Fabiha Tabassum Poroma** — completed as part of the Syntecxhub cybersecurity internship.