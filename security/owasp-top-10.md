---
title: "OWASP Top 10"
date: 2018-08-19T15:20:10-04:00
draft: false
description: The 10 most critical security vulnerabilities. 
tags: ['owasp', 'owasp top 10']
categories: ['security']

---



OWASP (Open Web Application Security Project) reports on the most critical security vulnerabilities found on websites. Knowing what's on the list can help you be aware of potential risks in your own systems and aid in establishing strategies to protect from those attacks. 



## The 2017 OWASP Top 10

#### Injection 

SQL injection attacks occur when an attack tries to get your application to execute malicious code designed to perform CRUD operations on your database. Areas of your site that accept user input and store it in the database are targets for SQL injection. That's why you should always use prepared statements for your SQL. 



#### Broken Authentication

A common security problem, is improperly implemented security. This can include broken user authentication that allows an attacker to gain access to another account. 



#### Sensitive Data Exposure

This may include unencrypted passwords, credit card numbers, social security number, etc. Sensitive data should always be encrypted in flight and at rest. 



#### XML External Entities (XEE) 

This is caused by XML parsers processing malicious XML. The exploit can gain information about the internal system or even execute remote code. 



#### Broken Access Control

Systems generally limit what users can and can't do. But if that system is broken, the sky is the limit. 



#### Security Misconfiguration

You may be getting the idea by now that a false sense of security is worse than an awareness of existing insecurity. So you thought you thought did everything right to secure your server. But whoops you let IPV6 wide open, even though you secured IPV4 like Fort Knox. 



#### Cross Site Scripting (XSS) 

Cross site scripting happens when an attacker is able to inject malicious  JavaScript into your website, or maybe it's just a malicious website  to begin with. Whatever the case, that JavaScript executes in the users browser and performs operations on their behalf. Cross site request forgery (CSRF) is a type of XSS. In this scenario a script might purchase something from another website that you left, but are still logged into.   That's why you should always use CSRF tokens for validatioen. Note: XSS is a good reason to be picky when using node modules ;) . 



#### Insecure Deserialization 

https://www.owasp.org/index.php/Deserialization_Cheat_Sheet

This would be along the lines of deserializing a JSON object on the backend. The recommended solution is, don't accept serialized objects from untrusted sources. 



#### Using Components with Known Vulnerabilities

So this would mean you have to check for known vulnerabilities and either get the security patches or stop using the vulnerable software.  There are websites dedicated to reporting vulnerabilities. 

https://www.npmjs.com/advisories

https://www.cvedetails.com



#### Insufficient Logging and Monitoring 

> Exploitation of insufficient logging and monitoring is the bedrock of nearly every major incident
>
> [OWASP](https://www.owasp.org/images/7/72/OWASP_Top_10-2017_%28en%29.pdf.pdf)

You should have good logging and altering in place so attacks are caught before an exploit actually takes place. 



In the security world there is the concept of [defense in depth](https://en.wikipedia.org/wiki/Defense_in_depth_(computing)) . You can use this methodology to help secure your system. 



##### References 

https://www.owasp.org/images/7/72/OWASP_Top_10-2017_%28en%29.pdf.pdf

