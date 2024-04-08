## [](#header-2) Enumeration / Recon Commands
resolve all ip addresses in a subnet range
```powershell
subnet.py [subnet range] | httpx -probe -sc -cl -td -fr | grep "SUCCESS"
```
