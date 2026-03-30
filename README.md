# Hacktify Cybersecurity Internship — Emmanuel Ateji

**Program:** HCS - Penetration Testing Internship  
**Platform:** [Hacktify Cybersecurity](https://hacktify.in)  

---

## Table of Contents

1. [About the Internship](#about-the-internship)
2. [Programme Structure](#programme-structure)
3. [Weekly Penetration Testing Labs](#weekly-penetration-testing-labs)
   - [Week 1 — HTML Injection & XSS](#week-1--html-injection--cross-site-scripting-xss)
   - [Week 2 — IDOR & SQL Injection](#week-2--insecure-direct-object-references-idor--sql-injection)
   - [Week 3 — CSRF & CORS Misconfiguration](#week-3--cross-site-request-forgery-csrf--cors-misconfiguration)
   - [Week 4 — CTF Challenges](#week-4--ctf-challenges)
4. [CTF Concepts Covered](#ctf-concepts-covered)
5. [Tools & Technologies Used](#tools--technologies-used)
6. [Skills Developed](#skills-developed)
7. [Repository Structure](#repository-structure)

---

## About the Internship

This repository documents my technical work completed during the **Hacktify Cybersecurity Penetration Testing (HCPT) 1-Month Internship**. The programme combined structured web application penetration testing lab exercises with Capture The Flag (CTF) challenges, producing formal black-box security assessment reports each week.

Each week's labs were documented in a structured penetration testing report covering vulnerability descriptions, payloads used, exploitation consequences, countermeasures, and proof-of-concept screenshots — mirroring real-world security assessment deliverables.

---

## Programme Structure

| Week | Lab Focus | Sub-labs | Difficulty Distribution |
|------|-----------|----------|------------------------|
| 1 | HTML Injection + Cross-Site Scripting | 17 | High: 4 / Medium: 4 / Low: 9 |
| 2 | IDOR + SQL Injection | 16 | High: 4 / Medium: 7 / Low: 5 |
| 3 | CSRF + CORS Misconfiguration | 13 | High: 5 / Medium: 4 / Low: 4 |
| 4 | CTF Challenges (Web, Forensics, Esoteric Lang.) | 6 challenges | Mixed |

---

## Weekly Penetration Testing Labs

### Week 1 — HTML Injection & Cross-Site Scripting (XSS)
 
**Scope:** `labs.hacktify.in/HTML/html_lab/` and `labs.hacktify.in/HTML/xss_lab/`

#### HTML Injection (6 Sub-labs)

| Sub-lab | Title | Risk | Key Technique |
|---------|-------|------|---------------|
| 1.1 | HTML's are easy! | Low | Injecting `<h1>` tags into a search field; discovered via browser inspector |
| 1.2 | Let me Store them! | Low | Stored HTML injection via profile input fields; password storage flaw also identified |
| 1.3 | File Names are also vulnerable! | Low | HTML injection via malicious filenames in file upload functionality |
| 1.4 | File Content and HTML Injection a perfect pair! | Medium | Uploaded `.html` file executing `document.cookie` script — combined file upload + HTML injection |
| 1.5 | Injecting HTML using URL | Medium | HTML/JS injection via URL parameters; payload: `<script>alert(document.cookie)</script>` + `<h1>` tag |
| 1.6 | Encode IT! | High | URL-encoded XSS payload bypassing input filters; encoded form: `%3Cscript%3E...%3C%2Fscript%3E` |

**Vulnerability class:** Improper input validation and output encoding — user-supplied data reflected into the HTML response without sanitisation. Consequences include webpage defacement, phishing form injection, session hijacking, and escalation to XSS.

**Countermeasures documented:** Output encoding (HTML entities), input whitelisting, Content Security Policy (CSP) headers, file type whitelisting, `htmlspecialchars()` and equivalent functions.

---

#### Cross-Site Scripting (11 Sub-labs)

| Sub-lab | Title | Risk | XSS Type | Key Technique |
|---------|-------|------|----------|---------------|
| 2.1 | Let's Do IT! | Low | Reflected | Basic `<script>alert()` in email subscription form |
| 2.2 | Balancing is Important in Life! | Low | Reflected | Payload breaks out of HTML attribute context using `"><script>` |
| 2.3 | XSS is everywhere! | Low | Reflected | Email field injection — `"><script>...@gmail.com` appended to bypass email format |
| 2.4 | Alternatives are must! | Low | Reflected | `<IMG SRC = q onerror=prompt(document.cookie)>` — event handler bypass |
| 2.5 | Developer hates scripts! | High | Reflected | `<IMG SRC=javascript:alert(1)>` to bypass `<script>` tag filters |
| 2.6 | Change the Variation! | High | Reflected | Whitespace-modified IMG tag payload to evade basic pattern-matching filters |
| 2.7 | Encoding is the key? | Medium | Reflected | URL-encoded payload decoded server-side prior to execution |
| 2.8 | XSS with File Upload (file name) | Low | Stored | XSS payload embedded in filename; executed when file listing is rendered |
| 2.9 | XSS with File Upload (file content) | Low | Stored | Uploaded `.html` file containing `<script>alert(document.cookie)</script>` |
| 2.10 | Stored Everywhere! | Low | Stored | XSS injected into first name, last name, and email profile fields; payload persisted to database |
| 2.11 | DOM's are love! | High | DOM-based | `coin` URL parameter read by `searchParams.get()` without sanitisation; `onerror` event handler triggers cookie access |

**Vulnerability class:** Insufficient input validation and output encoding across reflected, stored, and DOM-based XSS contexts. Payloads escalated from basic `<script>` tags through event-handler bypasses, file-upload-based stored XSS, and client-side DOM manipulation.

---

### Week 2 — Insecure Direct Object References (IDOR) & SQL Injection
 
**Scope:** `labs.hacktify.in/HTML/idor_lab/` and `labs.hacktify.in/HTML/sqli_lab/`

#### IDOR (4 Sub-labs)

| Sub-lab | Title | Risk | Key Technique |
|---------|-------|------|---------------|
| 1.1 | Give me my amount!! | Low | Manipulating `?id=` parameter in URL to access other users' financial and personal data |
| 1.2 | Stop polluting my params! | Medium | HTTP parameter pollution — `?id=1&id=2`; iterating IDs to enumerate user records |
| 1.3 | Someone changed my Password! | Medium | `?username=` parameter manipulation to reset a different user's password without authorisation |
| 1.4 | Change your methods! | Medium | Incrementing `?id=` (e.g., 1752 → 1753) to view and modify another user's profile |

**Vulnerability class:** Absence of server-side authorisation checks — the application trusts client-supplied object references without verifying whether the requesting user has permission to access the resource. Risks include unauthorised data access, account takeover, mass data enumeration, and regulatory violations (GDPR, PCI DSS).

---

#### SQL Injection (12 Sub-labs)

| Sub-lab | Title | Risk | Injection Type | Key Payload / Technique |
|---------|-------|------|----------------|------------------------|
| 2.1 | Strings & Errors Part 1! | Low | Auth bypass (string) | `admin" or "1"="1` in login form username field |
| 2.2 | Strings & Errors Part 2! | Low | Auth bypass (URL param) | `?id=1' +or+"1"="1"--+` |
| 2.3 | Strings & Errors Part 3! | Easy | Error-based | `?id=1'` triggers SQL syntax error; column enumeration via `ORDER BY 1--+` |
| 2.4 | Let's Trick 'em! | Medium | Auth bypass | `'="or'` creates tautology in WHERE clause |
| 2.5 | Booleans and Blind! | High | Boolean-based blind | `?id=1 OR 1=1` — consistent page output confirms injection |
| 2.6 | Error Based: Tricked | Medium | Error-based auth bypass | `') or ('a'='a' and hi") or ("a"="a` multi-quote tautology |
| 2.7 | Errors and Post! | Easy | POST-based auth bypass | `' or '1'='1` in email field of POST login form |
| 2.8 | User Agents lead us! | High | Header injection (User-Agent) | Modified `User-Agent: " OR "1"="1` intercepted and replayed via Burp Suite |
| 2.9 | Referer lead us! | Medium | Header injection (Referer) | `1" OR "1"="1` injected into the `Referer` HTTP request header |
| 2.10 | Oh Cookies! | High | Cookie-based UNION | `' union SELECT version(),user(),database()#` injected into `username` cookie |
| 2.11 | WAF's are injected! | High | UNION + WAF bypass | `?id=1&id=0' +union+select+1,@@version,database() --+` with HTTP parameter pollution to evade WAF signatures |
| 2.12 | WAF's are injected Part 2! | Medium | WAF bypass (integer param) | Integer `?id=9` retrieves plaintext credentials without triggering WAF rule |

**Vulnerability class:** Unsanitised user input incorporated directly into SQL queries — spanning login forms, URL parameters, HTTP headers (User-Agent and Referer), and cookie values. Attacks ranged from simple auth bypass to UNION-based data extraction and WAF evasion.

---

### Week 3 — Cross-Site Request Forgery (CSRF) & CORS Misconfiguration
 
**Scope:** `labs.hacktify.in/HTML/csrf_lab/` and `labs.hacktify.in/HTML/cors_lab/`

#### CSRF (6 Sub-labs)

| Sub-lab | Title | Risk | Key Technique |
|---------|-------|------|---------------|
| 1.1 | Eassyy CSRF | Easy | No CSRF token on password change form; malicious auto-submitting HTML form triggers cross-origin request |
| 1.2 | Always Validate Tokens | Medium | Static, predictable CSRF token (`"Always Validate Tokens!!!!"`) — present in form but not validated server-side |
| 1.3 | I hate when someone uses my tokens! | Medium | CSRF token not bound to user session — token from User A reused successfully in User B's context |
| 1.4 | GET Me or POST ME | Easy | POST-based password change with no anti-CSRF measures; application relies solely on cookies for authentication |
| 1.5 | XSS the saviour | Hard | XSS chained with CSRF — XSS payload bypasses same-origin restrictions to deliver and execute CSRF attack |
| 1.6 | rm -rf token | Hard | CSRF + command injection — predictable token; `rm -rf token` injected via password field, deleting the server-side token file |

---

#### CORS Misconfiguration (7 Sub-labs)

| Sub-lab | Title | Risk | Misconfiguration Type |
|---------|-------|------|----------------------|
| 2.1 | CORS with Arbitrary Origin | Easy | Server reflects any `Origin` header value in `Access-Control-Allow-Origin` with `Allow-Credentials: true` |
| 2.2 | CORS with Null Origin | Easy | Server permits `Origin: null` — exploitable via `<iframe>` with `data:` or `file:` scheme |
| 2.3 | CORS with Prefix Match | Medium | Origin validated by prefix only — attacker-controlled domain `hacktify.attacker.com` bypasses restriction |
| 2.4 | CORS with Suffix Match | Medium | Wildcard suffix match allows `*.attacker.com.hacktify.in` — attacker registers a matching subdomain |
| 2.5 | CORS with Escape Dot | Hard | Weak regex allows `wwwhacktify.in` (dot omitted) to match `www.hacktify.in` pattern |
| 2.6 | CORS with Substring Match | Hard | Origin validated for substring `hacktify.co` — `attacker.hacktify.co` bypasses control |
| 2.7 | CORS with Arbitrary Subdomain | Hard | Policy allows any subdomain of `hacktify.in` (e.g., `evil.hacktify.in`) with `Allow-Credentials: true` |

**Vulnerability class:** Overly permissive `Access-Control-Allow-Origin` policies combined with `Access-Control-Allow-Credentials: true` — attackers can issue authenticated cross-origin requests from malicious domains and read sensitive responses, enabling data exfiltration, session hijacking, and account takeover.

---

### Week 4 — CTF Challenges

**Platform:** Hacktify CTFd (`portal.hacktify.in/challenges`)

| Challenge | Category | Flag | Methodology |
|-----------|----------|------|-------------|
| The World | Web — Directory Enumeration | `FLAG{Y0u_hav3_4xpl0reD_th3_W0rLd!}` | DirBuster enumeration of target → discovered `secret.txt` → Base64 decoded flag |
| Lock Web | Web — Information Disclosure | `flag{v13w_r0b0t5.txt_c4n_b3_u53full!}` | DirBuster scan → `robots.txt` leaked `correctPin: "1928"` → entered PIN on keypad interface |
| Shadow Web | Network Forensics | `flag{mult1pl3p4rtsc0nfus3s}` | Wireshark PCAP analysis → 36+ HTTP POST requests each carrying one character in `multipart/form-data` body → assembled characters → Base64 decoded |
| Success Recipe | Esoteric Programming (Chef + Brainfuck) | `flag{yOu_40+_s3rv3d!}` | Recognised cooking recipe as Chef language → executed in Esolang Park → output contained Brainfuck code → decoded via md5decrypt.net Brainfuck Translator |
| Wh@t7he#### | Esoteric Programming (ReverseFuck) | `flag{R3v3rs3ddd_70_g3t_m3}` | Identified `+`, `-`, `<`, `>`, `,` symbol pattern as ReverseFuck → executed via dcode.fr interpreter |
| Corrupted | File Forensics — PNG Header Repair | `flag{m3ss3d_h3ad3r$}` | Corrupted PNG with broken file header → repaired via EaseUS Online Photo Repair → flag visible as text in restored image |

---

## CTF Concepts Covered

Beyond the challenge write-ups, the programme included structured material on:

- **What is a CTF?** — Competition structure, flag format conventions, point scoring based on difficulty and solve time, team vs. individual formats
- **CTF Challenge Categories** — Cryptography, Reverse Engineering, Web Exploitation, Forensics, Esoteric/Miscellaneous
- **PicoCTF** — Beginner-friendly practice platform; challenge navigation by difficulty category; progression approach at [picoctf.org](https://picoctf.org)
- **CTFd Platform** — Framework used to host CTF events; navigating challenge panels, scoreboards, team rankings, and flag submission workflows; flag format awareness (whitespace/blank space rejection pitfalls)
- **General CTF Strategy** — Start with lower-difficulty challenges, read post-solve write-ups, practice regularly on TryHackMe / Hack The Box / OverTheWire, collaborate in teams, think creatively and laterally

---

## Tools & Technologies Used

| Tool | Version | Purpose |
|------|---------|---------|
| OWASP DirBuster | 1.0-RC1 | Web directory and file enumeration |
| Burp Suite Community Edition | v2025.1.1 / v2025.1.2 | HTTP request interception, modification, and replay |
| Wireshark | Desktop | PCAP capture analysis and HTTP stream inspection |
| Kali Linux | Rolling | Primary penetration testing operating system |
| Browser DevTools (Inspector) | Built-in | DOM inspection, cookie review, source analysis |
| Hacktify CSRF PoC Generator | Web-based | Auto-generating CSRF proof-of-concept HTML pages |
| Esolang Park — Chef IDE | Web-based | Executing Chef esoteric language code |
| dcode.fr ReverseFuck Interpreter | Web-based | Decoding and executing ReverseFuck code |
| md5decrypt.net Brainfuck Translator | Web-based | Translating Brainfuck code to plaintext output |
| EaseUS Online Photo Repair | Web-based | Repairing corrupted PNG file headers |
| base64decode.org | Web-based | Decoding Base64-encoded strings |
| Online URL Encoder/Decoder | Web-based | Encoding and decoding URL payloads for filter bypass |
| GitHub | Version control | Source code and report repository management |

---

## Skills Developed

**Web Application Penetration Testing**  
Hands-on exploitation of OWASP Top 10 vulnerability classes across 46 structured sub-labs — HTML Injection, XSS (Reflected, Stored, DOM-based), IDOR, SQL Injection (error-based, boolean-blind, UNION-based, header injection, cookie-based, WAF evasion), CSRF, and CORS misconfiguration — all with formal security assessment reporting.

**Network Forensics**  
PCAP analysis in Wireshark — filtering HTTP traffic, following TCP streams, extracting character-level data from `multipart/form-data` POST requests to reconstruct hidden Base64-encoded messages.

**Esoteric Language Analysis**  
Identifying and executing code in Chef, Brainfuck, and ReverseFuck programming languages; recognising contextual clues (naming conventions, nonsensical instruction patterns) as language indicators.

**File Forensics**  
Identifying file header corruption in PNG files at the binary level and recovering image data using repair tooling.

**Encoding & Obfuscation**  
Working with Base64 and URL encoding both as attack vectors (filter evasion in injection payloads) and as forensic extraction and decoding methods in CTF scenarios.

**Penetration Testing Methodology & Reporting**  
Producing structured black-box security assessment reports for each finding, covering: vulnerability description, discovery method, affected URLs, proof of concept, exploitation consequences, recommended countermeasures, and references — consistent with industry reporting standards.

**CTF Problem-Solving**  
Approaching diverse challenge categories (web exploitation, network forensics, esoteric languages, file forensics) through systematic reconnaissance, appropriate tool selection, and creative lateral thinking.

---

## Repository Structure

```
/
├── reports/
│   ├── Week1_HTML_Injection_XSS_Report.pdf
│   ├── Week2_IDOR_SQLi_Report.pdf
│   ├── Week3_CSRF_CORS_Report.pdf
│   └── Week4_CTF_Report.pdf
└── README.md
```

---

**Author:** Emmanuel Ateji  
**Internship:** Hacktify Cybersecurity — HCPT Penetration Testing
