---
title: Open Redirect Vulnerability
description: Open Redirect Also known as Unvalidated Redirects & Forwards
date: 2026-01-01
categories: [01_Knowledge-Base, 02_Vulnerabilities]
tags: [Bug Bounty, z3nbu9 , Vulnerabilities, OpenRedirect]

---

## Introduction


**Open Redirect** Also known as Unvalidated Redirects & Forwards

Open redirects happen when the wed application takes an untrusted input and redirects a user from the web application to untrusted site or resources that will be used further for malicious purposes.

The impact for Open Redirect is usually low, unless you are using it to escalate other vulnerabilities.

Sometimes the application may have some security measures in place where the developers define a list of either trusted or untrusted resources. In some cases, you may be able to bypass it, if you fully understand how it works.

> https://example.com/login/?nextPage=https://google.com    **(allowed)**

> https://example.com/login/?nextPage=https://evilsite.com  **(Not allowed)**

> https://example.com/login/?nextPage=https://evilsite.com/?google.com   **(allowed)**


## Vulnerability

## Steps to Fix 

## Lab Examples

### nahamsec.training

#### or1.naham.sec (Low Level)

![]()
if you **click** Google to see the website.

url changes into 

> http://or1.nahamsec.trining/?redirect=http://www.google.com

If you change into 

> http://or1.nahamsec.trining/?redirect=http://www.evilsite.com

It redirects to `evilsite.com`

#### or2.naham.sec (Medium Level)

url

> http://or2.nahamsec.trining/?redirect=http://www.google.com  **(ALLOWED)**

If you change into 

> http://or1.nahamsec.trining/?redirect=http://www.evilsite.com  **(NOT ALLOWED)**

- Check for different types of places are works or not.

**Solution:**

Checks Which one that really working redirect

> http://or2.nahamsec.trining/?redirect=http://www.google.com  **(ALLOWED)**

With Redirect

> http://or2.nahamsec.trining/?redirect=http://www.google.com@evilsite.com  **(ALLOWED)**

Most of Web Browsers are redirect into domain which is after to `@` and ignores domain before  of `@`.