Port Scanner
===

Project Description
---
This project is a basic implementation of port scanner, which helps network administrators to ensure machines in their network run in normal. 

Basically, this port scanner can scan all ports of ip addresses given by user. By sending a proper packet to a port of a remote host, it can parse packets returned from that host which indicates the state of that port. Ip addresses are also needed during executing. The ip formats the port scanner can read are user input(8.8.8.8), ip file(like [this](https://github.com/OldPanda/Port-Scanner/blob/master/ip_list.txt)), and ip prefix(8.8.8.8/20). 

Scan Types
---
The port scanner can do multiple scans for a port. All following techniques are refered to [this link](http://nmap.org/book/man-port-scanning-techniques.html). 

* TCP SYN
* TCP FIN
* TCP NULL
* TCP XMAS
* TCP ACK
* UDP

Service Verification
---
In this project, we can also verify the services running on the specific ports. Based on the instruction, the services can be verified in this program are 

* SSH
* HTTP
* SMTP2
* POP
* IMAP
* WHOIS

Port scanner will send a query to those ports is they are required to be scanned. Then parsing the returned message to read the service running on that port. If no response from remote host, label that port as `Unable to connect`. For other port, just call functions in `net/db.h` to get the corresponding service name. 

Multithreading
---
To accelerate the scanning, multiple threads are supported in this project. By giving the number of threads following `--speedup`, the port scanner will run that number of threads at the same time. 

For this, we built a task queue to store all scan tasks. When call scan functions, it dequeues one task each time when calling a scan function(`tcp_scan()` or `udp_scan()`). 


Usage
---
```
./portScanner [option1, ..., optionN]
    --help                                	Print this help screen
    --ports <ports>                       	Set the ports to scan(default: 1-1024)
    --ip <IP address>                     	Set IP address to scan
    --prefix <IP prefix>                  	Set IP prefix to scan
    --file <file containing IP addresses> 	Read IP addresses from file
    --speedup <number of threads>         	Set how many threads to use
    --scan <scan types>                   	Set scan types(default: all scans)
```

Example
---
After executing `sudo ./portScanner --file ip_list.txt --ports 15,7,9,89,80 --scan UDP SYN --speedup 5`, the result of our code is 

```   
Scanning...
Scan took 12.60133 seconds.
IP address: 129.79.78.193
Port	Service Name (if applicable)	Scan Type	Results
------------------------------------------------------------------------------
80  	HTTP 1.1                   	SYN       	open
89  	su-mit-                    	SYN       	filtered
9   	discard                    	SYN       	filtered
7   	echo                       	SYN       	filtered
15  	netstat                    	SYN       	filtered
80  	HTTP 1.1                   	UDP       	open | filtered
15  	netstat                    	UDP       	open | filtered
7   	echo                       	UDP       	open | filtered
9   	discard                    	UDP       	open | filtered
89  	su-mit-                    	UDP       	open | filtered

IP address: 128.210.7.199
Port	Service Name (if applicable)	Scan Type	Results
-------------------------------------------------------------------------------
7   	echo                       	SYN       	closed
9   	discard                    	SYN       	closed
15  	netstat                    	SYN       	closed
80  	Unable to connect.         	SYN       	open
89  	su-mit-                    	SYN       	closed
7   	echo                       	UDP       	closed
9   	discard                    	UDP       	closed
15  	netstat                    	UDP       	closed
80  	Unable to connect.         	UDP       	closed
89  	su-mit-                    	UDP       	closed
```

The MIT License (MIT)
---

Copyright (c) 2014 Jinhui Zhang

Permission is hereby granted, free of charge, to any person obtaining a copy
of this software and associated documentation files (the "Software"), to deal
in the Software without restriction, including without limitation the rights
to use, copy, modify, merge, publish, distribute, sublicense, and/or sell
copies of the Software, and to permit persons to whom the Software is
furnished to do so, subject to the following conditions:

The above copyright notice and this permission notice shall be included in all
copies or substantial portions of the Software.

THE SOFTWARE IS PROVIDED "AS IS", WITHOUT WARRANTY OF ANY KIND, EXPRESS OR
IMPLIED, INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF MERCHANTABILITY,
FITNESS FOR A PARTICULAR PURPOSE AND NONINFRINGEMENT. IN NO EVENT SHALL THE
AUTHORS OR COPYRIGHT HOLDERS BE LIABLE FOR ANY CLAIM, DAMAGES OR OTHER
LIABILITY, WHETHER IN AN ACTION OF CONTRACT, TORT OR OTHERWISE, ARISING FROM,
OUT OF OR IN CONNECTION WITH THE SOFTWARE OR THE USE OR OTHER DEALINGS IN THE
SOFTWARE.

