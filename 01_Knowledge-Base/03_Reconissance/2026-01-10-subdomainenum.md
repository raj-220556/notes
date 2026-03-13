---
title: Reconnaissance and Subdomain Enumeration
description: 
date: 2026-01-10
categories: [01_Knowledge-Base, 02_Vulnerabilities]
tags: [Bug Bounty, z3nbu9 , subdomain, Enumeration, recon]

---

## **1. Reconnaissance and Subdomain Enumeration**

### **1.1 Passive Subdomain Enumeration**
**🛠️Tools:** [Subfinder](https://github.com/projectdiscovery/subfinder), [Amass](https://github.com/OWASP/Amass), [CRTSH](https://crt.sh/), [Github-Search](https://github.com/gwen001/github-search)

**Subfinder**
```bash
subfinder -d target.com -silent -all -recursive -o subfinder_subs.txt
```

**Amass (Passive Mode)**
```bash
amass enum -passive -d target.com -o amass_passive_subs.txt
```

**CRT.sh Query**
```bash
curl -s "https://crt.sh/?q=%25.target.com&output=json" | jq -r '.[].name_value' | sed 's/\*\.//g' | anew crtsh_subs.txt
```

**Github Dorking**
```bash
github-subdomains -d target.com -t YOUR_GITHUB_TOKEN -o github_subs.txt
```

**Results Combination**
```bash
cat *_subs.txt | sort -u | anew all_subs.txt
```

### **1.2 Active Subdomain Enumeration**

**🛠️Tools:** [MassDNS](https://github.com/blechschmidt/massdns), [Shuffledns](https://github.com/projectdiscovery/shuffledns), [DNSX](https://github.com/projectdiscovery/dnsx), [SubBrute](https://github.com/TheRook/subbrute), [FFuF](https://github.com/ffuf/ffuf)

**MassDNS**
```bash
massdns -r resolvers.txt -t A -o S -w massdns_results.txt wordlist.txt
```

**Shuffledns**
```bash
shuffledns -d target.com -list all_subs.txt -r resolvers.txt -o active_subs.txt
```

**DNSX Resolution**
```bash
dnsx -l active_subs.txt -resp -o resolved_subs.txt
```

**SubBrute**
```bash
python3 subbrute.py target.com -w wordlist.txt -o brute_force_subs.txt
```

**FFuF Subdomain**
```bash
ffuf -u https://FUZZ.target.com -w wordlist.txt -t 50 -mc 200,403 -o ffuf_subs.txt
```

### **1.3 Handling Specific (Non-Wildcard) Targets**

**🛠️Tools:** [GAU](https://github.com/lc/gau), [Waybackurls](https://github.com/tomnomnom/waybackurls), [Katana](https://github.com/projectdiscovery/katana), [Hakrawler](https://github.com/hakluke/hakrawler)

**GAU**
```bash
gau target.example.com | anew gau_results.txt
```

**Waybackurls**
```bash
waybackurls target.example.com | anew wayback_results.txt
```

**Katana**
```bash
katana -u target.example.com -silent -jc -o katana_results.txt
```

**Hakrawler**
```bash
echo "https://target.example.com" | hakrawler -depth 2 -plain -js -out hakrawler_results.txt
```

### **Additional Advanced Techniques**

**🛠️Tools:** [CloudEnum](https://github.com/initstring/cloud_enum), [AWSBucketDump](https://github.com/jordanpotti/AWSBucketDump), [S3Scanner](https://github.com/sa7mon/S3Scanner)

**Reverse DNS**
```bash
dnsx -ptr -l resolved_subs.txt -resp-only -o reverse_dns.txt
```

**ASN Enumeration**
```bash
amass intel -asn <ASN_NUMBER> -o asn_results.txt
```

**Cloud Asset Enumeration**
```bash
cloud_enum -k target.com
```

**Results Validation**
```bash
cat all_subs.txt | httpx -silent -title -o live_subdomains.txt
```

---

<br>