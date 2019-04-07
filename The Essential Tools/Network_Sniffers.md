## Network Sniffers

Network Sniffers are programs that capture low-level package data that is transmitted over a network. An attacker can analyze this information to discover valuable information such as user ids and passwords.

Network sniffing is the process of intercepting data packets sent over a network. This can be done by the specialized software program or hardware equipment. Sniffing can be used to;

- Capture sensitive data such as login credentials
- Eavesdrop on chat messages
- Capture files have been transmitted over a network

The following are protocols that are vulnerable to sniffing. They send all the data as plain text and are susceptible to MITM(Man-in-the-middle) attack.

Telnet
Rlogin
HTTP
SMTP
NNTP
POP
FTP
IMAP

Download Wireshark from this link http://www.wireshark.org/download.html

- Open Wireshark
- Select the network interface you want to sniff. if you are using a wireless network connection chose wlan0. If you are on a local area network, then you should select the local area network interface, oscp candidates need to choose tap0 as the local Network
- Open your browser to start capturing.

After you record some network data, it's time to take a look at the captured packets.
The captured data interface contains three main sections:

### Packet List Pane
- The first pane displays a list containing packets in the current capture file. Its displayed as a table and the columns contain: the packet number, the time captured, packet source and destination, packet’s protocol, and some general information found in the packet.
Packet Details Pane
- The second pane contains a hierarchical display of information about a single packet. Click the “collapsed and expanded” to show all of the information collected about an individual packet.
Packet Bytes Pane
- The third pane contain encoded packet data, displays a packet in its raw, unprocessed form.

- STOP CAPTURING AND SAVE TO A .PCAP FILE if you want.

Wireshark Filters:

There are two kinds of filters which each have its own functionality: Capture filter and Display filter.

1. CAPTURE FILTER

Capture filter is used to capture specific data or packets, it is used in “Live Capture Session”, for example you only need to capture single host traffic on 192.168.1.10 . So, input the query to the Capture filter form:

host 192.168.1.10

The main benefit of using Capture filter is that we can reduce the amount of data in the captured file, because instead of capturing any packet or traffic, we specify or limit to certain traffic. Capture filter controls what type of data in traffic will be captured, if no filter is set, it means capture all. To configure capture filter, click Capture Options button

You will notice Capture Filter Box in the bottom, click on the green icon beside the box and select the filter you want.

2. DISPLAY FILTER

Display filter, in other hand, is used in “Offline Analyzing”. Display filter is more like a search feature of certain packets you want to see on the main window. Display filter controls what is seen from an existing packet capture, but does not influence what traffic is actually captured. You can set display filter during capturing or analyzing. You will notice the Display Filter box in the top of the main window. Actually there are so many filters you can apply, but don’t be overwhelmed. To apply a filter you can either just type a filter expression inside the box, or select from the existing list of available filters.

Click Expressions button beside Display Filter box to choose available filters.

Suppose, you want to see only http traffic in the captured packets, then specify in the display filter box:

http

Now, if you want to see login credentials which someone typed in the http website during your capture window, search for http POST request.

Click on POST request. Now goto the packet details panel just below this window,  Look for the summary that says Line-based text data: application/x-www-form-urlencoded, and there you will see plain text username and password.

Tutorial:
https://www.lifewire.com/wireshark-tutorial-4143298
https://www.guru99.com/wireshark-passwords-sniffer.html
https://www.comparitech.com/net-admin/how-to-use-wireshark/
video - https://www.youtube.com/watch?v=TkCSr30UojM

TCPDUMP:

tcpdump is the network analysis tool, this is a very essential tool for analyzing packets when you don't have any GUI available for wireshark.

use command to see raw packets : tcpdump -nnvSX port <port_no>

Cheatsheet
https://danielmiessler.com/study/tcpdump/
https://www.andreafortuna.org/2018/07/18/tcpdump-a-simple-cheatsheet/
