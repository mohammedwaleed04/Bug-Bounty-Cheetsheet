## [](#header-2) Enumeration / Recon
Burp Regex for Scope Control
```powershell
.*\.example\..*$
```
Reverse Whois Search
```powershell
https://www.whoxy.com/example.com
```
Resolve IP Addresses In A Subnet Range
```powershell
subnet.py [subnet range] | httpx -probe -sc -cl -td -fr | grep "SUCCESS"
```
Subdomain Enumeration Commands
```powershell
subfinder -d example.com -all
assetfinder -subs-only example.com
amass enum -d example.com -active
amass enum -d example.com -passive
```
Subdomain Bruteforce
```powershell
~/Desktop/tools/subbrute/subbrute.py example.com
~/Desktop/tools/subbrute/subbrute.py -s ~/Desktop/tools/massdns/lists/names.txt example.com
```
Subdomain Permutations
```powershell
cat domains.txt | dnsgen - | dnsx -silent
sed 's/A.*//' livesubs | sed 's/CN.*//' | sed 's/\..$//' > domains.resolved
```
Extract A records for the given list of subdomains
```powershell
dnsx -l testsubs -silent -a -resp-only
```
HTTPX Command
```powershell
httpx -l final -probe -sc -cl -td -ip -fr -p 80,443,8080 | grep "SUCCESS"
```
