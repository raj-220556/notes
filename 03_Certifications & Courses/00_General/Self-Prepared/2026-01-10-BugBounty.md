---
title: Bug Bounty Course
description: In This Course You Learn Things that pay for you!
date: 2026-01-10
categories: [03_Certifications & Courses, 00_General]
tags: [Bug Bounty, z3nbu9]

---


## Introduction To Bug Bounty

### What is a Bug Bounty Program?

A program where ethical hacers are invited to report security vulnerabilities to organization, in exchange for monetary rewards for useful submissions.

Bug bounties are commonly seen as the most effective and inexpensive way to identify vulnerabilities in live systems and products.

### Popular Bug Bounty Platforms
- [bugcrowd](https://www.bugcrowd.com/) : First bugbounty platform
- [hackerone](https://www.hackerone.com/)
- [zerocopter](https://www.zerocopter.com/)
- [Cobalt](https://www.cobalt.io/)
- [Synack](https://www.synack.com/)

You Find BugBounty programs in **Programs** Section on platforms.

### Benifits Of Bug Bounty

- Access to thousands of security researchers with diverse skill set
- Incentives for quality and severity of Bugs.
- Very competitive environment. THe one who reports the bug first gets the reward.
- Pricing is based Pay Per Bug (PPB) Model.
- Bug Bounty helps in attaining a good understanding of your security and the weaknesses of your network


### What bugs are in scope?

- [SQL Injection]()
- [Cross Site Scripting(XSS)]()
- [Insecure Direct Object References(IDOR)]()
- [Cross Site Request Forgery(CSRF)]()
- [Security Misconfiguration]()
- [Insecure Cross-Origin Resource Sharing(CORS)]()
- [Brute Forcing]()
- [URL Redirection]()
- [File Inclusion]()
- [Server Side Request Forgery]()


--- 

## Hacking Terminologies

**Vulnerability** : It is defined as a flaw that exist in a system which the developer of the system unaware of.

**Exploit** : An exploit is the means by which an attacker, or penetration tester for takes advantage of a vulnerability within a system, an application, or a service. An attacker uses an exploit to attack a system in a way that results in a particular desired outcome that the developer never excepted.

**Payload** : It is a customized code that attacker need the system to execute and that's to be chosen and delivered by the Framework.

**Phishing** : It is the fraudulent attempt to obtain sensitive information such as usernames, passwords and credit card details by disguising oneself as a trustworthy entity in an electronic communication.

**Zero Day Attack** : It is simply the use of a previously undiscovered flaw in an application or operation system that can be exploited to gain access to or control system resources. The term zero-day refers to the fact that it is the day on which the attack or exploit was first identified.


--- 

## Information Gathering

**What is Information Gathering?**
Gathering as much information about the target as possibile and organizing it in a structured manner so that it can be utillized later in the vulnerability assessment and penetration testing (VAPT) phase.

**What is Reconnaissance?**
It is procees of analyzing all the gathered information & utilizing it to understand the target.

**Digital Footprinting**
- They are the traces left online when a person is using the internet.
- They can be used to trace a person or a web application. <br>
For example: 
   - IP Address left Online
   - Likes on Instagram/FB images
   - Prefrences and address entered while shopping online

### What Information to gather?

- Critical assets or web applications that belong to the client
- Related domains and subdomains
- Registration details of each domain.(Owner,developer,hosting,etc.)
- Server architecture of the application running on these domains
- Other web applications running on the same server as the target domains
- Critical cached pages
- Older snapshots of the web applications

### What is Whois Information?

**Whois Information** : When someone buys a domain name, a database stores all the registration details.

It is a protocol that receives response from the database that stores registration information of a domain or an IP address.

The details include : 
1. Name of the organization who the domain belongs to
2. Owner of the person purchasing the domain
3. Name of the developer who handles it
4. Full name, complete address, contact number, email address etc.

<https://whois.domaintools.com/>

> **NOTE** : Whois cannot be used as a proof of ownership as the registrant can fill any bodies or hoax information and Whois is not checked while registering a domain/IP.
{: .prompt-warning }

### Gathering Information About Websites

Getting an idea about the technology being used by websites and web server
: 

>  [www.builtwith.com](https://builtwith.com/)

1. Important sections Frameworks: To see the programming languages used.
2. Hosting providers: To see where the website is hosted.
3. Web Server: To see the server software being used.


Going through the history of a website 
: 

To see how the website looked in the past its features additions and deletions that have been
made over time.

> [web.archive.org](https://web.archive.org/)

1. Go to the year you want to see
2. Check out screenshots taken on an day, and also see the website as it was on that day

Finfing out sub domains related to a domain
: 

> [www.dnsdumpster.com](https://dnsdumpster.com/)

> [www.virustotal.com](https://www.virustotal.com/)

1. Host Records (A): To see a list of all sub domains of any given domain


### Google Dorking and GHDB

**Dorks** : They are special kewords that you can use to do a more accurate search on google, bing, yahoo, etc.

List of Google Dorks is as follow:
: 
- **allintext** : searches for specific text contained on any web page.
  - ex: `allintext: hacking tools`
- **allinurl** : It can be used to fetch results whose URL contains all the specified characters.
  - ex: `allinurl: client area`
- **filetype** : used to search for any kind of file extensions
  - ex: `filetype: jpg`
- **intitle** : used to search for various keywords inside title
  - ex: `intitle: security tools` will search for titles beginning with "security" but "tools" can be somewhere else in the page.
- **intext** : useful to locate pages that contain certain characters or strings inside their text.
  - ex: `intext: "safe internet"`



#### GHDB - Google Hacking Database

This a dark mine, which contain thousands of dorks in it.

<https://www.exploit-db.com/google-hacking-database>

Various vulnerable servers, IOT devices relevant information can be find using these dorks.

### Information Gathering about People

Information About people can be : Full name, Email address, Mobile number etc.

Methods of Finding Full name and Email Address
1. Social Media Platforms - FB, instagram, Twitter, Snapchat, Pinterest, YouTube, Hangout etc.
2. Professional Platform - Linkedin, Naukri.com, Glassdoor.com etc.
3. Google Search - Most powerful search engine & if used correctl can be of good use.

- Finding Name behind Phone number use TrueCaller, Forgot Password in Shoping Websites, Google Search with operators

### Organization Information Finding

Ways to gather information are:
1. Social Media Platforms
2. Company Review Services
3. Orgainzation Financial Analysis Services

Social Media Platforms
: 
Any organization has official or unofficial accounts on social media platforms like FB, twitter, etc. These can be used to gather information about activities of the organization also find out about the current or ex employees.

Company Review Services
: 
Many websites like Timesjobs, Glassdoor etc. provide company Review service. There current and ex-employees provide inside information about there organization.

This informationcould be salary structure , environment, turn over etc.

Organization Financial Analysis
: 

Some example of website providing financial analysis services are: 
1. Crunchbase
2. Wikipedia
3. AngelList

They provide information about the financial situation of organization & key people associated with it.

--- 



## HTTP Basics

### The Anatomy of an HTTP Request

```http
GET /index.html HTTP/1.1
Host: www.example.com
User-Agent: Mozilla/5.0
Accept: text/html
Authorization: Bearer SOMETHING_HERE
Referrer: https://www.google.com

```

- What page (or endpoint) to fetch `GET /index.html HTTP/1.1`
- What website (or host) to fetch from `Host: www.example.com`
- Browser information including name, version and etc `User-Agent: Mozilla/5.0`.
- What type of data to send/receive `Accept: text/html`
- Authorization header allowing you to fetch data `Authorization: Bearer SOMETHING_HERE`
- What site/page sent you to this new page `Referrer: https://www.google.com`


### Commonly Used HTTP Methods 

- **GET** to fetch data
- **HEAD** does the same thing as get but doesn't show the full response
- **POST** to create or change data
- **PUT** to replace or modify data
- **DELETE** to delete data
- **OPTIONS** to see communications options (GET, HEAD, POST, PUT, DELETE, ETC)

### Most Common Response Status Codes

- 200 range - Successful response
- 300 range - Redirects
- 400 range
  - 401 - Unauthorized or Unauthenticated
  - 403 - Forbidden or no access to resources
  - 404 - Not found or file doesn't exist
  - 405 - HTTP Method not allowed
- 500 - Internal error where the server doesn't know how to handle the request
- And tons more. You can Check them out Mozill's website

> (<https://developer.mozilla.org/en-US/docs/Web/HTTP/Status>)



## Setting Up Labs

https://github.com/nahamsec/nahamsec.training



## Vlunerabilities

### Open Redirect

**Open Redirect** Also known as Unvalidated Redirects & Forwards

Open redirects happen when the wed application takes an untrusted input and redirects a user from the web application to untrusted site or resources that will be used further for malicious purposes.

The impact for Open Redirect is usually low, unless you are using it to escalate other vulnerabilities.

Sometimes the application may have some security measures in place where the developers define a list of either trusted or untrusted resources. In some cases, you may be able to bypass it, if you fully understand how it works.

> https://example.com/login/?nextPage=https://google.com    **(allowed)**

> https://example.com/login/?nextPage=https://evilsite.com  **(Not allowed)**

> https://example.com/login/?nextPage=https://evilsite.com/?google.com   **(allowed)**

For more... with lab example<br>
[Open Redirect](/posts/openredirect)


### Cross-Site Scripting (XSS)

#### What is XSS?

XSS allows an attacker to execute arbitary client-side code on a victim's browser. XSS can be used for phishing, exfiltrating data, account takeovers and more.

#### How does XSS work?

- An attacker inserts a malicious script (or a payload) into the victim's browser
- When the victim encounters the script it executes in the victim's browser
- A malicious payload is able to perform any action that the victim is able to perform.
- If the victim has special privileges it can be a serious vulnerability.

#### Impact

- Read / Modify / Delete content of any page
- Steal a users cookies or session and gain access to their account
- Serve malicious content like phishing


#### Types of XSS

##### Reflected XSS

Payload is injection from the victim's request. Victim must click a malicious link or navigate to an attacker-controlled property.

#### Stored XSS 

Payload is stored server-side and can be triggered by a victim with no interaction outside of the application

#### DOM XSS

The Vulnerability is in the client-side code versus the server-sider. Injection is still typically from the victim's request.

For more... with lab example<br>
[Cross-Site Scripting (XSS)](/posts/xss)

### Cross-Site Request Forgery (CSRF)

### Insecure Direct Object Reference (IDOR)

### Local File Disclosure (LFD)

### SQL Injection

### Server Side Request Forgery (SSRF)

### XML External Entity (XXE)

### Remote Command Execution (RCE)

### Testing File Uploaders

### Recon






## Report Writing