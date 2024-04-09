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
Subdomain Enumeration One Line Command
```powershell
(subfinder -d example.com && assetfinder -subs-only example.com && amass enum -passive -d example.com) | sort -u > domains.txt
```
Subdomain Permutation Bruteforce
```powershell
cat out | dnsgen - | httpx -probe -sc -cl -td -ip -fr | grep "SUCCESS"
```
