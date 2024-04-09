## [](#header-2) Enumeration / Recon
Burp Regex for Scope Control
```powershell
.*\.frontapp\..*$
```
Reverse Whois Search
```powershell
https://www.whoxy.com/[domain]
```
Resolve IP Addresses In A Subnet Range
```powershell
subnet.py [subnet range] | httpx -probe -sc -cl -td -fr | grep "SUCCESS"
```
Subdomain Enumeration Commands
```powershell
subfinder -d [domain] -all
assetfinder -subs-only [domain]
amass enum -d [domain] -active
amass enum -d [domain] -passive
```
Subdomain Permutation Bruteforce
```powershell
cat domains.txt | dnsgen - | massdns -r ~/Desktop/tools/massdns/lists/resolvers.txt -t A -o S -w livesubs
sed 's/A.*//' livesubdomains.messy | sed 's/CN.*//' | sed 's/\..$//' > domains.resolved
```
