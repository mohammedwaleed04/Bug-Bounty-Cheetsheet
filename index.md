## [](#header-2) Enumeration / Recon Commands
resolve ip addresses in a subnet range
```powershell
subnet.py [subnet range] | httpx -probe -sc -cl -td -fr | grep "SUCCESS"
```
reverse whois search
```powershell
https://www.whoxy.com/[domain]
```
subdomain enumeration one line command
```powershell
(subfinder -d example.com && assetfinder -subs-only example.com && amass enum -passive -d example.com) | sort -u > domains.txt
```
Burp Regex for Scope Control
```powershell
.*\.frontapp\..*$
```
