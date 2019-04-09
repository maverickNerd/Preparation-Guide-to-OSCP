## Active Information Gathering

This is the most important phase of information gathering where you directly involve with the target.
Don't do this before taking prior permission from client.

It is illegal without telling the other party about what you are doing.

I will cover lot of tools in this section so that you can get information about your target.

### Domain Name System (DNS)

Understanding of DNS will help to know how actually internet works, when you type in domain name like
www.google.com, what happens behind the scene.

- DNS is responsible for resolving the domain name to IP address. IP addresses are the unique address that computers understand.

#### Domain Name

Domain name is easy to remember name that we type in the url to access the resourse
like www.google.com is the domain name.

DNS resolves this domain name into IP address which is the address of the google server which serves our request.

If you want to understand DNS terminology and how it works, please
go through following article:

https://www.digitalocean.com/community/tutorials/an-introduction-to-dns-terminology-components-and-concepts

### Security Concerns
Some viruses and other malware programs can change your default DNS server to a DNS server run by a malicious organization or scammer. This malicious DNS server can then point popular websites to different IP addresses, which could be run by scammers.

For example, when you connect to facebook.com while using your Internet service provider’s legitimate DNS server, the DNS server will respond with the actual IP address of Facebook’s servers.

However, if your computer or network is pointed at a malicious DNS server set up by a scammer, the malicious DNS server could respond with a different IP address entirely. In this way, it’s possible that you could see “facebook.com” in your browser’s address bar, but you may not actually be at the real facebook.com. Behind the scenes, the malicious DNS server has pointed you to a different IP address.

To avoid this problem, ensure that you’re running good antivirus and anti-malware apps. You should also watch for certificate error messages on encrypted (HTTPS) websites. For example, if you try to connect to your bank’s website and see an “invalid certificate” message, this could be a sign that you’re using a malicious DNS server that’s pointing you to a fake website, which is only pretending to be your bank.

## DNS Enumeration

There are many tools available in kali, which can be used to get name server and mail server information about the domain.

We might be able to do zone transfer also which can expose both internal as well as external network of the domain.

DNS Zone transfer is the process where a DNS server passes a copy of part of it's database (which is called a "zone") to another DNS server. It's how you can have more than one DNS server able to answer queries about a particular zone; there is a Master DNS server, and one or more Slave DNS servers, and the slaves ask the master for a copy of the records for that zone.

A basic DNS Zone Transfer Attack isn't very fancy: you just pretend you are a slave and ask the master for a copy of the zone records. And it sends you them; DNS is one of those really old-school Internet protocols that was designed when everyone on the Internet literally knew everyone else's name and address, and so servers trusted each other implicitly.

It's worth stopping zone transfer attacks, as a copy of your DNS zone may reveal a lot of topological information about your internal network. In particular, if someone plans to subvert your DNS, by poisoning or spoofing it, for example, they'll find having a copy of the real data very useful.

So best practice is to restrict Zone transfers. At the bare minimum, you tell the master what the IP addresses of the slaves are and not to transfer to anyone else. In more sophisticated set-ups, you sign the transfers. So the more sophisticated zone transfer attacks try and get round these controls

#### host

- To know name servers of a domain:
root@kali:/home/greysec/scripts# host -t ns zonetransfer.me
zonetransfer.me name server nsztm2.digi.ninja.
zonetransfer.me name server nsztm1.digi.ninja.

- To know mail server of a domain:
root@kali:/home/greysec/scripts# host -t mx zonetransfer.me
zonetransfer.me mail is handled by 20 ASPMX4.GOOGLEMAIL.COM.
zonetransfer.me mail is handled by 0 ASPMX.L.GOOGLE.COM.
zonetransfer.me mail is handled by 20 ASPMX3.GOOGLEMAIL.COM.
zonetransfer.me mail is handled by 10 ALT1.ASPMX.L.GOOGLE.COM.
zonetransfer.me mail is handled by 20 ASPMX5.GOOGLEMAIL.COM.
zonetransfer.me mail is handled by 20 ASPMX2.GOOGLEMAIL.COM.
zonetransfer.me mail is handled by 10 ALT2.ASPMX.L.GOOGLE.COM.

For zone transfer:

root@kali:/home/greysec/scripts# host -l zonetransfer.me nsztm2.digi.ninja.
Using domain server:
Name: nsztm2.digi.ninja.
Address: 34.225.33.2#53
Aliases:

zonetransfer.me has address 5.196.105.14
zonetransfer.me name server nsztm1.digi.ninja.
zonetransfer.me name server nsztm2.digi.ninja.
14.105.196.5.IN-ADDR.ARPA.zonetransfer.me domain name pointer www.zonetransfer.me.
asfdbbox.zonetransfer.me has address 127.0.0.1
canberra-office.zonetransfer.me has address 202.14.81.230
dc-office.zonetransfer.me has address 143.228.181.132
deadbeef.zonetransfer.me has IPv6 address dead:beaf::
email.zonetransfer.me has address 74.125.206.26
home.zonetransfer.me has address 127.0.0.1

### DnsEnum

Using dnsenum tool, you can get nx,mx record and it tries to do zonetransfer with a single command:

root@kali:/home/greysec/scripts# dnsenum zonetransfer.me

### DnsRecon

With this tool, we can:

- Query all the available DNS records
- Brute force for subdomains A
- Attempt Zone Transfer attacks against every NS record

root@kali:/home/greysec/scripts# dnsrecon -d zonetransfer.me


For more tools -
https://resources.infosecinstitute.com/dns-enumeration-techniques-in-linux/#gref

# Port Scanning(Nmap)

Ports are the virtual socket that your application uses for communication purpose.
It's a tool used for port scanning.

WHAT IS NMAP?
Nmap or Network mapper is an open source tool for network discovery and security analysis. It is used by many people in different job roles, from system administrators to penetration testers to developers. The primary uses are network discovery and analysis.

Nmap uses raw IP packets to determine what hosts are available on a network, it can also be used to identify what services (application name and version), operating system versions, what filters/firewalls are in use, and dozens of other characteristics.

Nmap runs on all major computer operating systems, and official binary packages are available for Linux, Windows, and Mac OS X. Typically nmap is used on the command line by calling nmap however there is also a GUI available in the form of zenmap.

In addition to the core tooling the nmap suite also includes a netcat-like tool on steroids(ncat), scan results comparison (Ndiff), and a packet generation and response analysis tool (Nping).

## PORT SCANNING

Port scanning is the act systematically scanning a computer's ports/services. Since a port is a place where information goes in and out of a computer, port scanning identifies openings into a computer. Port scanning has legitimate uses in managing networks, system administration and other network based tasks. However it can also be malicious in nature if someone is looking for a weak access point to break into a system.

Typically it is one of the first techniques used to identify weaknesses or footholds into a network. One thing to note though is that the act of port scanning does fall under active recon and will send traffic to a target rather than passive scanning using things like OSINT.

## PORT STATES

Before we dive into the different flags, it is worth understanding that when scanning a port can have three states and depending on the scan type will depend on why the state has been returned. The main three are:

- Open: Open means that an application or service is listening for connections or traffic on the target system.
- Closed: Closed ports have no application listening on them.
- Unfiltered: Ports are classified as unfiltered when they are responsive to Nmap's probes, but Nmap cannot determine whether they are open or closed.
- Filtered: Filtered means that a firewall, filter, or other network obstacle is blocking the port so that Nmap cannot tell whether it is open or closed.

Nmap reports other state combinations such as open|filtered and closed|filtered when it cannot determine which of the two states describe a port.

### SOME COMMON COMMANDS
Nmap is one of the most used tools when carrying out infrastructure-like engagements. As such there are many different flags and command combinations that can be used to identify weaknesses and interesting information about hosts. The following sets of commands can be used to scan different types of hosts, each flag is explained and has been tuned for maximum performance.

### BASIC SCANNING OPTIONS
There are three fairly commmon flags used in nmap for types of scanning, these are TCP connect scans, SYN & UDP. The flags for are shown below and a brief explanation of how they work is included too:

-sT: TCP connect scan is the default TCP scan type when SYN scan is not an option. This is selected when a user doesn't have elevate priveleges on a machine and therefore does not have permission to send raw packets or is scanning IPv6 networks. Instead of writing raw packets as most other scan types do, Nmap asks the underlying operating system to establish a connection with the target machine and port by issuing the connect system call and a TCP three way handshake. This is the same high-level system call that web browsers, and most other network-enabled applications use to establish a connection.

-sS: This flag is a SYN scan and it is the default, most popular scan option when using nmap. It can be performed quickly, scanning thousands of ports per second on a fast network or modern network. SYN scaning is relatively unobtrusive and stealthy, since it does not complete the TCP handshake, rather it sends SYN and waits for a SYN-ACK response. Based on the response the port will then come back as either open, closed or filtered:

ProbeResponse	                                    Assigned State

TCP SYN/ACK response	                            open

TCP RST response	                                closed

No response received or ICMP unreachable errors	  filtered

-sU: UDP scan works by sending a UDP packet to every targeted port. For most ports, this packet will be empty (no payload), but for a few of the more common ports a protocol-specific payload will be sent. Based on the response, or lack thereof, the port is assigned to one of four states as detailed in the table below:

Probe Response	                                                   Assigned State

Any UDP response from target port	                                 open

No response received after retransmission	                         open|filtered

ICMP port unreachable error (type 3, code 3)	                     closed

Other ICMP unreachable errors (type 3, code 1, 2, 9, 10, or 13)	   filtered

-sn - Ping scan, used for sweeping the network, it sends ICMP packets to know if the host is up or not.

-sV: Version scan, this will probe specific services and try to identify what version of a particular application or service is running on that port.

-O: OS Scan, this will send additional probes in order to determine what operating system the host is likely running.

### PROBING (-P<X>)
There are so many different options when it comes to probing a service however here are some of the specifics when it comes to probing things.

-Pn: Don't ping the host, assume it's up - this is useful for hosts that don't respond to or block ping requests.

-PB: Default probing, scan port 80,443 and send an ICMP to the target.

-PE: Use a default ICMP echo request to probe a target

-PP: Use an ICMP timestamp request

-PM: Use an ICMP network request

### DEFAULT TIMING OPTIONS (-TX)
Sometimes when tuning a scan you might want to have certain options set to speed up or slow down scanning depending on if you want to be noisy or stealthy!

-T5: Insane; Very aggressive timing options, gotta go fast! This will likely crash unstable networks so shy away from it, in some instances it will also miss open ports due to the level of aggression.

-T4: Aggressive; Assumes a stable network, may overwhelm some networks if not setup to cope.

-T3: Normal; A dynamic timing mode which is based on how responsive the target is.

-T2: Polite; Slows down to consume less bandwidth, runs roughly ten times slower than a normal scan.

-T1: Sneaky; Quite slow, used to evade IDS and stay quiet on a network

-T0: Paranoid; Very slow, used to evade IDS and stay almost silent on a network.

In addition to these options you can fine tune a scan even more with the particular settings most people use these options to speed nmap up, but they can also be useful for slowing Nmap down. Often people will do that to evade IDS systems, reduce network load, or even improve accuracy if network conditions are so bad that even nmap's conservative default is too aggressive., these flags are detailed in the following table:

Function	                                             Flags

Size of the group of hosts to be scanned concurrently	--min-hostgroup, --max-hostgroup

Number of scanning probes to be launched in parallel	--min-parallelism, --max-parallelism

Timeout values for probes	                            --min-rtt-timeout, --max-rtt-timeout, --initial-rtt-timeout

Maximum number of probe retransmissions allowed	      --max-retries

Maximum time before giving up on an entire host	      --host-timeout

Control the delay inserted between each probe against an individual host	--scan-delay, --max-scan-delay

Rate of probe packets sent per second	                --min-rate, --max-rate

Defeat RST packet response rate by target hosts	      --defeat-rst-ratelimit

### OUTPUTS
Viewing the output in realtime can be useful however parsing the information afterwards and feeding it into other tools is much more useful. You can use different output options from nmap, which can save to a file of one sort or another.

There's a few options to output to but mainly these are xml,gnmap & nmap and have the flags; -oX, -oG, -oN but there is also an easter egg output in 1337 speak which is -oS.

-oX: This instructs nmap to give the output in XML format for parsing later.

Basic example: nmap 10.0.0.1 -oX outFile

-oG: To do the same thing for a grepable file, -oG can be used. If I wanted to pull up the text from that file, I can use: grep
HTTPS output.gnmap. This will search that file for the phrase HTTPS and output the result.

Basic example: nmap 10.0.0.1 -oG outFile

-oN: nmap output will be the same as what is shown in realtime when you're running a scan, it allows you to quickly identify open
ports or the bigger picture about a target.

Basic example: nmap 10.0.0.1 -oN outFile

-oS: This option serves no real value over the past three however will output the results in a leet speak format for a bit of fun.

-oA: Lastly to output to all the formats previously mentioned ( .nmap, .gnmap, .xml ) just give it a name and you're away.

Basic example: nmap 10.0.0.1 -oA outFile

Another useful output type is to view stats on the running scan. An example would be: nmap –stats-every 25s 10.0.0.1 to show me the statistical information every 25 seconds during a scan. You can use s for seconds, m for minutes, or h for hours for this scan. This can be done to reduce the amount of info filling up a screen.

### SCANNING A SINGLE HOST FOR TOP 1000 OPEN PORTS
By default nmap scan for top 100 ports, these 100 ports are the most commonly used ports which nmap thinks are useful for any scan.

nmap -sT <host> --top-ports 1000 -oA TCP-Top1000

This command essentially does the following:

nmap : This is the name of the tool in use, nmap

-sT : This flag tells nmap to do a full TCP Connect scan against the target.

<host> : This is where the host goes either domain(google.com) or IP address(84.92.52.67)

--top-ports 1000 : This tells nmap to scan the top 1000 ports

-oA : Output the results to .gnmap,.nmap & .xml

TCP-Top1000 : Name of output file

#### SOME OPTIONS I USE

nmap -sSV -p- --min-parallelism 64 --min-hostgroup 16 --max-hostgroup 64 --max-retries 3 -Pn -n -iL input_hosts.txt -oA output --verson-all  --reason

The different flags in this command do the following:

-sSV: This conducts a syn scan with version checks included.

-p-: Tells nmap to scan all 65535 ports (1 - 65535), if you want to include port 0 you'll need to do -p 0-65535.

--min-parallelism 64: Launch 64 parallel tasks to probe the target.

--min-hostgroup 16: Scan a minimum of 16 hosts at one time and...

--max-hostgroup 64: ... a maximum amount of 64 hosts.

--max-retries 3: The amount of times to retry probing a port before moving onto the next service.-Pn: Skip ping scans, assume the
host is up.

-n: Skip dns resolution, usually select this when not interested in reverse dns or wanting a quicker scan :-).

-iL input_hosts.txt: Take an input file containing target hosts.

-oA output: Output the results to .gnmap,.nmap & .xml for parsing later and analysing.

--verson-all: Do extended version checks against the host to find out services running.

--reason: Detail the reason why a port is determined as open, filtered or closed.

Probing a specific service for more information and looking for known issues:

sudo nmap -sSV --version-all -p 11211 --min-parallelism 64 --script=vuln 10.0.0.1 -Pn -n

The addition of the --script=vuln and specifc port tells nmap to only probe the port 11211 and tell me any vulnerable services it knows about running on that port. Additionally -sC can be used to scan a target and probe with common scripts. More information on the scripting engine can be found below.

One final one-liner I use a lot is to get the output of a subnet mask, something like:

nmap -sL -n 10.10.10.1/24 | grep report | cut -d " " -f 5 >>  ips.txt

This will simply print all of the hosts in the range given as individual IP addresses, very useful when you don't have a subnet calculation on hand or want unique ips for other tools!

Nmap Reference Guide - https://nmap.org/book/man.html
Nmap Cheatsheet: https://blogs.sans.org/pen-testing/files/2013/10/NmapCheatSheetv1.1.pdf

## Nmap Scripting Engine (NSE)

The Nmap Scripting Engine (NSE) is one of Nmap's most powerful and flexible features. It allows users to write (and share) simple scripts to automate a wide variety of networking tasks.
NSE scripts can be used to enumerate the target, as well as to find vulnerabilites.

Nmap Scripting Engine (NSE): https://nmap.org/book/man-nse.html

You can use NSE script with following options of nmap command:
-sC
Performs a script scan using the default set of scripts. It is equivalent to --script=default. Some of the scripts in this category are considered intrusive and should not be run against a target network without permission.

--script filename|category|directory|expression[,...]

kali:~# nmap 10.0.0.12 --script smb-os-discovery.nse


## FTP Enumeration (port 21)

- Fingerprint server
telnet ip_address 21 (Banner grab)

- Run command-
ftp ip_address

- Check for anonymous access
ftp ip_address
Username: anonymous OR anonPassword: any@email.com

- Password guessing
- Cracking Password
Hydra brute force
hydra -l USERNAME -P /usr/share/wordlist -f 192.168.X.XXX ftp -V


## SSH Enumeration (port 22)

- Fingerprint server
telnet ip_address 22 (banner grab)

- Password guessing
ssh root@ip_address

- Password Cracking
Hydra brute force

- Examine configuration files
ssh_config
sshd_config
authorized_keys
ssh_known_hosts

## Telnet port 23 open
- Fingerprint server
telnet ip_address

- Password Attack
Common passwords
Hydra brute force

## Sendmail Enumeration (Port 25)

- Fingerprint server
telnet ip_address 25 (banner grab)

#### Mail Server Testing
- Enumerate users
VRFY username (verifies if username exists - enumeration of accounts)
EXPN username (verifies if username is valid - enumeration of accounts)

- Mail Spoof Test
HELO anything MAIL FROM: some_address RCPT TO:some_address DATA . QUIT


## NetBIOS/SMB Enumeration (Ports 135-139,445)
Windows XP and Windows 2000 was effected by NULL session authentication issue, which can be exploited to get users info, password policies, group info and domain info.

- NetBIOS enumeration
nbtscan -r 10.1.1.0/24
enum <-u username> <-p password> <-f dictfile> <hostname|ip>

- Null Session (Windows)
net use \\192.168.1.1\ipc$ "" /u:""
net view \\ip_address

- Smbclient (Linux)
smbclient -L //server/share password options

nmblookup -A target

smbclient //MOUNT/share -I target -N

rpcclient -U "" target

enum4linux -a target

- Find Open SMB shares

Use NSE script:
nmap -T4 -v -oA shares --script smb-enum-shares --script-args smbuser=username,smbpass=password -p445 10.1.1.0/24

Find OS version:
nmap -v -p 139, 445 --script=smb-os-discovery 10.1.1.22


- Find SMB Users:

nmap -sU -sS --script=smb-enum-users -p U:137,T:139 10.1.1.200-254

Find vulnerability in SMB:

nmap -v -p 139,445 --script=smb-vuln* --script-args=unsafe=1 10.1.1.22

## SNMP Enumerate (port 161)

- Default Community Strings
public
private

- snmpwalk
snmpwalk -v <Version> -c <Community string> <IP>

eg: snmpwalk -c public -v1 192.168.1.X 1

snmpcheck -t 192.168.1.X -c public

- SNMP Bruteforce
onesixtyone
onesixytone -c SNMP.wordlist <IP>

hydra -P /usr/share/wordlist 192.168.XX.XX smtp -V


Cheatsheet:
http://0daysecurity.com/penetration-testing/enumeration.html
https://highon.coffee/blog/penetration-testing-tools-cheat-sheet/
