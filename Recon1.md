# Reconnaissance Report: hamropatro.com

## Overview
This document records the reconnaissance and enumeration activities performed on the domain **hamropatro.com** using tools available in Kali Linux. The objective was to identify subdomains, directories, and publicly accessible resources that may expand the attack surface.


---

# 1. Subdomain Enumeration

## Tool Used
Subdomain enumeration was performed using **Sublist3r**.

Tool: Sublist3r  
Purpose: Discover subdomains using OSINT and search engines.

### Command

```bash
sublist3r -d hamropatro.com
```

### Process
The tool queried multiple public sources including:

- Google
- Bing
- Yahoo
- Baidu
- Ask
- Netcraft
- DNSdumpster
- VirusTotal
- PassiveDNS
- SSL Certificate Transparency

### Output

```
Total Unique Subdomains Found: 7

www.hamropatro.com
app.hamropatro.com
english.hamropatro.com
health.hamropatro.com
l.hamropatro.com
recharge.hamropatro.com
remit.hamropatro.com
```

### Observation

7 subdomains were discovered:

| Subdomain | Possible Function |
|-----------|------------------|
| www.hamropatro.com | Main website |
| app.hamropatro.com | Application services |
| english.hamropatro.com | English language interface |
| health.hamropatro.com | Health related services |
| l.hamropatro.com | Possibly shortened links |
| recharge.hamropatro.com | Recharge/payment services |
| remit.hamropatro.com | Remittance services |

### Errors Observed

```
[!] Error: Virustotal probably now is blocking our requests
[!] DNSDumpster module failed: Could not find CSRF token
```

These errors indicate API blocking or site protection mechanisms.

---

# 2. Subdomain Enumeration using Amass

## Tool Used
Amass

Purpose:
- Attack surface mapping
- DNS enumeration
- Infrastructure discovery

### Command

```bash
amass enum -active -d hamropatro.com
```

### Activity During Execution

The tool attempted to download required datasets including:

- Address expansion datasets
- Address parser datasets
- Language classifier datasets

These datasets are part of the **libpostal library** used by Amass for data analysis.

### Observation

The process ended prematurely:

```
Killed
```

Possible reasons:

- Insufficient system memory
- High CPU usage
- Resource limitation

---

# 3. Attempted Tool Execution: rsearch

### Command

```bash
rsearch -u hamropatro.com
```

### Result

```
Command 'rsearch' not found
```

The tool was not installed on the system.

---

# 4. Directory Enumeration

Directory brute forcing was performed to discover hidden web directories.

## Tool Used

Dirsearch

Purpose:
- Discover hidden directories
- Identify exposed files
- Detect misconfigurations

### Installation

```bash
sudo apt install dirsearch
```

### Dependencies Installed

- python3-requests-ntlm
- python3-ntlm-auth

---

## Directory Scan Command

```bash
dirsearch -u hamropatro.com
```

### Scan Configuration

```
Extensions: php, aspx, jsp, html, js
HTTP Method: GET
Threads: 25
Wordlist Size: 11460
```

### Target

```
http://hamropatro.com/
```

---

# 5. Discovered Paths

The scan identified several paths that returned HTTP responses.

```
/axis//happyaxis.jsp
/axis2-web//HappyAxis.jsp
/axis2//axis2-web/HappyAxis.jsp
/Citrix//AccessPlatform/auth/clientscripts/cookies.js
/engine/classes/swfupload//swfupload_f9.swf
/engine/classes/swfupload//swfupload.swf
/extjs/resources//charts.swf
/html/js/misc/swfupload//swfupload.swf
```

### HTTP Status Code

```
301 Redirect
```

Example:

```
/axis//happyaxis.jsp -> https://www.hamropatro.com/axis/happyaxis.jsp
```

Meaning:
The server redirected requests to another location.

---

# 6. Scan Report Location

Dirsearch automatically saved the report:

```
/home/kali/reports/_hamropatro.com/_26-03-15_04-37-58.txt
```

---

# 7. Key Findings

| Category | Result |
|--------|--------|
Target Domain | hamropatro.com |
Subdomains Found | 7 |
Directory Scan | Completed |
HTTP Responses | 301 Redirect |
Enumeration Tools | Sublist3r, Amass |
Directory Scanner | Dirsearch |

---

# 8. Security Importance

Reconnaissance helps security researchers:

- Identify attack surfaces
- Discover hidden services
- Map infrastructure
- Detect exposed endpoints
- Understand system architecture

These steps are typically part of the **information gathering phase in penetration testing**.

---

# 9. Limitations

Some limitations encountered during the process:

- API blocking from VirusTotal
- DNSDumpster CSRF token failure
- Amass terminated due to resource limitations

These issues are common during automated reconnaissance.

---

# 10. Conclusion

The reconnaissance activity successfully discovered several subdomains and accessible paths associated with **hamropatro.com**.

This information helps security analysts:

- Understand the external attack surface
- Identify potential entry points
- Plan further vulnerability assessment activities

Future steps may include:

- Port scanning
- Technology fingerprinting
- Vulnerability scanning
- Web application testing

---

# Tools Used

Kali Linux  
Sublist3r  
Amass  
Dirsearch
