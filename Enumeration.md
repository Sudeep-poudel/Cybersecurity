# Reconnaissance & Scanning Report: daraz.com.np

## Objective
To perform reconnaissance and scanning on the target domain **daraz.com.np** using DNS lookup, WHOIS, and Nmap to gather network and service information.

---

# 1. DNS Enumeration (nslookup)

## Command Used

```bash
nslookup daraz.com.np
```

## Output

```
Server:         103.86.56.2
Address:        103.86.56.2#53

Non-authoritative answer:
Name:   daraz.com.np
Address: 47.246.167.168
Address: 47.246.174.23
Address: 47.246.174.148
Address: 47.246.167.152
```

## Analysis

- Multiple IP addresses were found.
- This indicates **load balancing or CDN usage**.
- The domain is hosted on distributed infrastructure.

---

# 2. WHOIS Lookup (Domain)

## Command Used

```bash
whois daraz.com.np
```

## Output

```
This TLD has no whois server
https://register.com.np/whois-lookup
```

## Analysis

- `.np` domains do not support standard WHOIS queries.
- Must use the official Nepal registry website.

---

# 3. WHOIS Lookup (IP Address)

## Command Used

```bash
whois 47.246.167.152
```

## Key Information

- Organization: Alibaba Cloud LLC
- Country: United States
- Network Range: 47.235.0.0 - 47.246.255.255
- ASN: Alibaba Infrastructure

## Analysis

- The target is hosted on **Alibaba Cloud infrastructure**.
- Indicates large-scale cloud hosting and global distribution.

---

# 4. Basic Port Scanning (Nmap)

## Command Used

```bash
nmap 47.246.167.152
```

## Output

```
PORT    STATE SERVICE
80/tcp  open  http
443/tcp open  https
```

## Analysis

- Only **HTTP (80)** and **HTTPS (443)** ports are open.
- All other ports are filtered → strong firewall/security.

---

# 5. Full Port Scan with Service Detection

## Command Used

```bash
nmap -sC -sV -p- 47.246.167.152
```

## Output Summary

```
PORT    STATE SERVICE  VERSION
80/tcp  open  http     Tengine httpd Aserver
443/tcp open  ssl/http Tengine httpd Aserver
```

### Additional Findings

- Server: **Tengine (Alibaba web server)**
- Redirect:
  ```
  http://www.taobao.com/
  ```
- HTTPS Error:
  ```
  500 Internal Server Error
  ```

## SSL Certificate Info

- Common Name: *.daraz.com
- Organization: Alibaba (China) Technology Co., Ltd.
- Validity:
  - Start: 2026-03-10
  - Expiry: 2027-04-11

## Analysis

- Uses **Tengine web server** (Alibaba customized Nginx).
- SSL certificate supports multiple Daraz domains.
- Strong infrastructure with centralized certificate management.

---

# 6. Aggressive Scan with OS Detection

## Command Used

```bash
nmap -A -T4 daraz.com.np
```

## Output Summary

```
PORT    STATE SERVICE  VERSION
80/tcp  open  http     Tengine httpd Aserver
443/tcp open  ssl/http Tengine httpd Aserver
```

## Additional Findings

- Multiple IPs:
  ```
  47.246.167.168
  47.246.174.23
  47.246.174.148
  47.246.167.152
  ```

- OS Detection:
  ```
  No OS matches found
  ```

- Network Distance:
  ```
  14 hops
  ```

---

## Traceroute Summary

```
1   192.168.1.1
2   10.44.60.1
3   103.86.x.x
...
14  47.246.167.168
```

## Analysis

- Traffic passes through multiple network nodes.
- Indicates global CDN/cloud routing.
- OS detection failed due to filtering → strong security.

---

# 7. Key Findings

| Category | Result |
|--------|--------|
Domain | daraz.com.np |
Hosting | Alibaba Cloud |
IP Addresses | 4 |
Open Ports | 80, 443 |
Web Server | Tengine |
Security | High (filtered ports) |
SSL | Valid wildcard certificate |

---

# 8. Security Insights

- Only essential ports are open → **reduced attack surface**
- Use of CDN/cloud → **high availability & protection**
- Firewall filtering → **blocks scanning attempts**
- Centralized SSL → **secure communication**

---

# 9. Limitations

- WHOIS for `.np` domain not available via CLI
- OS detection failed due to filtering
- Some responses redirected or blocked

---

# 10. Conclusion

The reconnaissance and scanning of **daraz.com.np** shows that the system is hosted on a secure and scalable cloud infrastructure (Alibaba Cloud). The target uses strong security practices such as port filtering, HTTPS encryption, and CDN-based distribution, making it resistant to basic scanning techniques.

---

# Tools Used

- Kali Linux
- nslookup
- whois
- nmap
