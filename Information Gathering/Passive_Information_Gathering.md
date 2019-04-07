### Passive Information Gathering

- Passive information gathering can be done using google or any other search engine.

- It can be any information that is available publicly, eg, you can gather emails of employee from linkedin, any information available on blogs/articles.

- If it is web penetration job, make sure to check their website, check  what kind of products they sell, is there any job vacancy available, if there is a web developer vacancy, check the skills that they are looking, it will most probably contain the technology their website is based upon.

### Google Hacking Database/Google Dork

I think everyone should learn how to use google efficiently.

Google has some very good search operators to find information that is not readily available on a website. These are called google dorks.

Google hacking database contains thousands of these filters which can help an information security person to find misconfiguration.

https://www.exploit-db.com/google-hacking-database

The basic syntax for advanced operators in Google is:

operator_name:keyword

Make sure there is no space between operator_name and keyword.

Some of the great google dorks which I use are:

site – will return website on following domain

allintitle and intitle – contains title specified phrase on the page

inurl – restricts the results contained in the URLS of the specified phrase

filetype – search for specified filetype formats

What Data Can We Find Using Google Dorks?

Admin login pages
Username and passwords
Vulnerable entities
Sensitive documents
Govt/military data
Email lists
Bank account details and lots more

if you want to list all the website that have exact phrase. search for a string like "access denied for user".

Cheatsheet: https://www.sans.org/security-resources/GoogleCheatSheet.pdf

## Email Harvesting

- Email harvesting can be done to find emails containing the organization name.

- This can be use for social engineering attacks, by sending spams/malware links to the email ID and penetrating the network using this technique or affecting the internal network.

- Emails of employees from the corporate company also helps in making the username list as most of the time username is part of email id,

#### theharvester

This tool can help in finding the emails belonging to an organization.

root@kali:/home/greysec/scripts# theharvester -d cisco.com -b linkedin

## Netcraft

Netcraft can be used to indirectly find out information about web servers on the Internet, including the underlying operating system, web server version, and uptime.

https://www.netcraft.com/

## Whois Enumeration

Whois is a name for a TCP service, a tool, and a type of database. Whois databases contain name server, registrar, and, in some cases, full contact information about a domain name. Each registrar must maintain a Whois database containing all contact information for the domains they host.

http://whois.domaintools.com/

root@kali:/home/greysec/scripts# whois flipkart.com

   Domain Name: FLIPKART.COM
   Registry Domain ID: 1009529768_DOMAIN_COM-VRSN
   Registrar WHOIS Server: whois.godaddy.com
   Registrar URL: http://www.godaddy.com
   Updated Date: 2016-06-02T16:11:34Z
   Creation Date: 2007-06-03T19:32:20Z
   Registry Expiry Date: 2022-06-03T19:32:20Z
   Registrar: GoDaddy.com, LLC

- You can see from the above output that flipkart domain is registered on godaddy.com and its creation and expiry date. Sometimes we can get the contact number and email id of internal organization also which can help in social engineering attack.

## Recon-ng

Recon-ng is a full-featured Web Reconnaissance framework written in Python. Complete with independent modules, database interaction, built in convenience functions, interactive help, and command completion, Recon-ng provides a powerful environment in which open source web-based reconnaissance can be conducted quickly and thoroughly.

Recon-ng has a look and feel similar to the Metasploit Framework, reducing the learning curve for leveraging the framework.

tutorial: https://www.youtube.com/watch?v=CM--WaOQEqo

### Usage:

root@kali:/home/greysec/scripts# recon-ng
recon-ng][default] > use recon/domains-contacts/whois_pocs
[recon-ng][default][whois_pocs] > set SOURCE microsoft.com
SOURCE => microsoft.com
[recon-ng][default][whois_pocs] > run

-------------
MICROSOFT.COM
-------------
[*] URL: http://whois.arin.net/rest/pocs;domain=microsoft.com

[*] URL: http://whois.arin.net/rest/poc/AADLA11-ARIN

[*] [contact] CHRIS AADLAND (v-chrisa@microsoft.com) - Whois contact

[*] URL: http://whois.arin.net/rest/poc/AADLA-ARIN

[*] [contact] CHRISTINA AADLAND (v-chrisa@microsoft.com) - Whois contact

[*] URL: http://whois.arin.net/rest/poc/AADLA1-ARIN

[*] [contact] CHRISTINA AADLAND (v-chrisa@microsoft.com) - Whois contact

[*] URL: http://whois.arin.net/rest/poc/AADLA10-ARIN
