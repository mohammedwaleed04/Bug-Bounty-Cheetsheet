## [](#header-2) Enumeration / Recon
Burp Regex for Scope Control
```powershell
.*\.example\..*$
```
Find Emails
```powershell
https://phonebook.cz/
```
Reverse Whois Search
```powershell
https://www.whoxy.com/example.com
```
Find Domains Hosted On The Same IP Address
```powershell
https://api.hackertarget.com/reverseiplookup/?q=example.com
```
Pulling root domains
```powershell
Whoxy Reverse Whois API

cat asn.txt | tlsx -san -cn -silent -resp-only | grep -v "example.com"

amass intel -asn 3389

https://crt.sh
cat crt | jq -r '.[].common_name' | sed 's/\*\.\(.*\)/\1/' | rev | cut -d "." -f 1,2 | rev | grep -v " " | sort -u
```
Resolve IP Addresses In A Subnet Range
```powershell
subnet.py [subnet range] | httpx -probe -sc -cl -td -fr | grep "SUCCESS"
xargs -a asn -I {} bash -c 'subnet.py {} | httpx -l final -probe -title -sc -cl -td -ip -fr -p 80,443,8080,8888,8000,8008 | grep "SUCCESS"'
```
Extract A records for the given list of subdomains
```powershell
dnsx -l testsubs -silent -a -resp-only
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
HTTPX Command
```powershell
httpx -l final -probe -title -sc -cl -td -ip -fr -p 80,443,8080,8888,8000,8008 | grep "SUCCESS"
```
Find Important Keywords In A List Of Subdomains Or Urls
```powershell
isubs.sh file.txt
```
