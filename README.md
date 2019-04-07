# Guide to OSCP

I am writing this guide to cover all OSCP topics as well as other infosec knowledge in details, I will also provide a cheat-sheet in each section so that you can use the commands directly once you understand the topics/tools. But I think to become a good pentester you should know how things work. Pentesting is a very wide field, like if you are interested in webapp pentesting then you should know how application interacts with databases, basics of databses, webapp languages.

Similarly if you want to become system/platform(OS) pentester, you should know how to setup the OS, how to do system configuration, and what people can miss when they do configuration. This can guide you to think in the direction of finding vulnerability.

If you want to go to network pentesting, you should know TC/IP layer, OSI layer, how to setup network interface and basic knowledge of how internet and firewall works.

Always, remember information security is a journey, there is never ending things to learn, so only thing that you need to conquer this journey is patience and curiosity.


When I started OSCP, I didn't even know about nmap, so it is not that before attempting OSCP you have to learn something.

Only prerequisite is:
- Basic understanding of OS, and TCP/IP protocol
- Also, it would be great if you are familiar with basic commands in linux and windows.
- Able to read and understand code written in C, python and bash

First phase of pentesting is the "Information Gathering" phase, in this the pentester searches for publicly available information about the client and identifies potential ways to connect to its systems. After this, the tester uses this information to determine the value of each finding and the impact to the client if
the finding permitted an attacker to break into a system.

Second phase is "Vulnerability Analysis", this is the phase where you attempt to discover vulnerability in the system, system can be a webapp, any application running on particular OS or the OS itself.
Once you find that there is a vulnerability, then you need to find a way to exploit it.

Next comes "Exploitation Phase" depends on the type/kind of vulnerability and a pentester need to craft a exploit using his/her skill to exploit the software.

Fourth phase is "Post-Exploitation" phase, where the result of the exploitation is used to find additional information, sensitive data(like password files, password stored in browser, other private files ), access to other systems resources.

Then comes the last phase called "Reporting" phase, in this, the pentester summarizes the findings, including steps of exploitation and problem with the software which allows attacker to exploit it.


I am covering all these phases including some other resource that will help you in OSCP as well as pentesting journey.

## Content
- [Setting Up Kali Linux](https://github.com/blackinfo/Preparation-Guide-to-OSCP/tree/master/Setting%20up%20Kali%20Linux)
