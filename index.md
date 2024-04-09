## [](#header-2) Enumeration / Recon
Resolve IP Addresses In A Subnet Range
```powershell
subnet.py [subnet range] | httpx -probe -sc -cl -td -fr | grep "SUCCESS"
```
Reverse Whois Search
```powershell
https://www.whoxy.com/[domain]
```
Subdomain Enumeration One Line Command
```powershell
(subfinder -d example.com && assetfinder -subs-only example.com && amass enum -passive -d example.com) | sort -u > domains.txt
```
Burp Regex for Scope Control
```powershell
.*\.frontapp\..*$
```
Subdomain Permutation Bruteforce
```powershell
cat out | dnsgen - | httpx -probe -sc -cl -td -ip -fr | grep "SUCCESS"
```
