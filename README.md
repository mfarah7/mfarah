<h1>DNS & HTTP</h1>





<h2> DNS & HTTP traffic log </h2>
<b>
This reading explains how to identify the brute force attack using tcpdump. 


14:18:32.192571 IP your.machine.52444 > dns.google.domain: 35084+ A? yummyrecipesforme.com. (24)

14:18:32.204388 IP dns.google.domain > your.machine.52444: 35084 1/0/0 A 203.0.113.22 (40)


The first section of the DNS & HTTP traffic log file shows the source computer (your.machine.52444) using port 52444 to send a DNS resolution request to the DNS server (dns.google.domain) for the destination URL (yummyrecipesforme.com). Then the reply comes back from the DNS server to the source computer with the IP address of the destination URL (203.0.113.22). 




14:18:36.786501 IP your.machine.36086 > yummyrecipesforme.com.http: Flags [S], seq 2873951608, win 65495, options [mss 65495,sackOK,TS val 3302576859 ecr 0,nop,wscale 7], length 0

14:18:36.786517 IP yummyrecipesforme.com.http > your.machine.36086: Flags [S.], seq 3984334959, ack 2873951609, win 65483, options [mss 65495,sackOK,TS val 3302576859 ecr 3302576859,nop,wscale 7], length 0


The next section shows the source computer sending a connection request (Flags [S]) from the source computer (your.machine.36086) using port 36086 directly to the destination (yummyrecipesforme.com.http). The .http suffix is the port number; http is commonly associated with port 80. The reply shows the destination acknowledging it received the connection request (Flags [S.]). The communication between the source and the intended destination continues for about 2 minutes, according to the timestamps between this block (14:18) and the next DNS resolution request (see below for the 14:20 timestamp). 


TCP Flag codes include:


Flags [S]  - Connection Start 
Flags [F]  - Connection Finish 
Flags [P]  - Data Push
Flags [R]  - Connection Reset
Flags [.]  - Acknowledgment

14:18:36.786589 IP your.machine.36086 > yummyrecipesforme.com.http: Flags [P.], seq 1:74, ack 1, win 512, options [nop,nop,TS val 3302576859 ecr 3302576859], length 73: HTTP: GET / HTTP/1.1




The log entry with the code HTTP: GET / HTTP/1.1 shows the browser is requesting data from yummyrecipesforme.com with the HTTP: GET method using HTTP protocol version 1.1. This could be the download request for the malicious file. 



14:20:32.192571 IP your.machine.52444 > dns.google.domain: 21899+ A? greatrecipesforme.com. (24)

14:20:32.204388 IP dns.google.domain > your.machine.52444: 21899 1/0/0 A 192.0.2.172 (40)

14:25:29.576493 IP your.machine.56378 > greatrecipesforme.com.http: Flags [S], seq 1020702883, win 65495, options [mss 65495,sackOK,TS val 3302989649 ecr 0,nop,wscale 7], length 0

14:25:29.576510 IP greatrecipesforme.com.http > your.machine.56378: Flags [S.], seq 1993648018, ack 1020702884, win 65483, options [mss 65495,sackOK,TS val 3302989649 ecr 3302989649,nop,wscale 7], length 0


Then, a sudden change happens in the logs. The traffic is routed from the source computer to the DNS server again using port .52444 (your.machine.52444 > dns.google.domain) to make another DNS resolution request. This time, the DNS server routes the traffic to a new IP address (192.0.2.172) and its associated URL (greatrecipesforme.com.http). The traffic changes to a route between the source computer and the spoofed website (outgoing traffic: IP your.machine.56378 > greatrecipesforme.com.http and incoming traffic: greatrecipesforme.com.http > IP your.machine.56378). Note that the port number (.56378) on the source computer has changed again when redirected to a new website.


Resources for more information
An introduction to using tcpdump at the Linux command line: Lists several tcpdump commands with example output. The article describes the data in the output and explains why it is useful.
tcpdump Cheat Sheet: Lists tcpdump commands, packet capturing options, output options, protocol codes, and filter options
What is a computer port? | Ports in networking: Provides a short list of the most common ports for network traffic and their associated protocols. The article also provides information about ports in general and using firewalls to block ports.
Service Name and Transport Protocol Port Number Registry: Provides a database of port numbers with their service names, transport protocols, and descriptions
How to Capture and Analyze Network Traffic with tcpdump?: Provides several tcpdump commands with example output. Then, the article describes each data element in examples of tcpdump output. 
Masterclass – Tcpdump – Interpreting Output: Provides a color-coded reference guide to tcpdump output 

</b>



<h2>DNS & HTTP traffic log</h2>

- <b>DNS & HTTP traffic log:</b> 14:18:32.192571 IP your.machine.52444 > dns.google.domain: 35084+ A? yummyrecipesforme.com. (24)
14:18:32.204388 IP dns.google.domain > your.machine.52444: 35084 1/0/0 A 203.0.113.22 (40)


14:18:36.786501 IP your.machine.36086 > yummyrecipesforme.com.http: Flags [S], seq 2873951608, win 65495, options [mss 65495,sackOK,TS val 3302576859 ecr 0,nop,wscale 7], length 0
14:18:36.786517 IP yummyrecipesforme.com.http > your.machine.36086: Flags [S.], seq 3984334959, ack 2873951609, win 65483, options [mss 65495,sackOK,TS val 3302576859 ecr 3302576859,nop,wscale 7], length 0
14:18:36.786529 IP your.machine.36086 > yummyrecipesforme.com.http: Flags [.], ack 1, win 512, options [nop,nop,TS val 3302576859 ecr 3302576859], length 0
14:18:36.786589 IP your.machine.36086 > yummyrecipesforme.com.http: Flags [P.], seq 1:74, ack 1, win 512, options [nop,nop,TS val 3302576859 ecr 3302576859], length 73: HTTP: GET / HTTP/1.1
14:18:36.786595 IP yummyrecipesforme.com.http > your.machine.36086: Flags [.], ack 74, win 512, options [nop,nop,TS val 3302576859 ecr 3302576859], length 0
…<a lot of traffic on the port 80>... 


14:20:32.192571 IP your.machine.52444 > dns.google.domain: 21899+ A? greatrecipesforme.com. (24)
14:20:32.204388 IP dns.google.domain > your.machine.52444: 21899 1/0/0 A 192.0.2.17 (40)

14:25:29.576493 IP your.machine.56378 > greatrecipesforme.com.http: Flags [S], seq 1020702883, win 65495, options [mss 65495,sackOK,TS val 3302989649 ecr 0,nop,wscale 7], length 0
14:25:29.576510 IP greatrecipesforme.com.http > your.machine.56378: Flags [S.], seq 1993648018, ack 1020702884, win 65483, options [mss 65495,sackOK,TS val 3302989649 ecr 3302989649,nop,wscale 7], length 0
14:25:29.576524 IP your.machine.56378 > greatrecipesforme.com.http: Flags [.], ack 1, win 512, options [nop,nop,TS val 3302989649 ecr 3302989649], length 0
14:25:29.576590 IP your.machine.56378 > greatrecipesforme.com.http: Flags [P.], seq 1:74, ack 1, win 512, options [nop,nop,TS val 3302989649 ecr 3302989649], length 73: HTTP: GET / HTTP/1.1
14:25:29.576597 IP greatrecipesforme.com.http > your.machine.56378: Flags [.], ack 74, win 512, options [nop,nop,TS val 3302989649 ecr 3302989649], length 0
…<a lot of traffic on the port 80>... 

<h2>Security incident report</h2>

- <b>Security incident report:</b> Security incident report

Section 1: Identify the network protocol involved in the incident
The protocol impacted in the incident is Hypertext transfer protocol (HTTP). Running tcpdump and accessing the yummyrecipesforme.com website to detect the problem, capture protocol, and traffic activity in a DNS & HTTP traffic log file provided the evidence needed to come to this conclusion. The malicious file is observed being transported to the users’ computers using the HTTP protocol at the application layer.





Section 2: Document the incident
The baker executed a brute force attack to gain access to the web host yummyrecipesforme.com. They repeatedly entered several known default passwords for the administrative account until they correctly guessed the right one. After they obtained the login credentials, they were able to access the admin panel and change the website’s source code. They embedded a javascript function in the source code that prompted visitors to download and run a file upon visiting the website. After running the downloaded file, the customers are redirected to a fake version of the website where the seller’s recipes are now available for free.




Section 3: Recommend one remediation for brute force attacks
First change your default admin password to something that isn’t weak and after that hash your password so it’s hard to guess by a threat actor.










<!--
 ```diff
- text in red
+ text in green
! text in orange
# text in gray
@@ text in purple (and bold)@@
```
--!>
