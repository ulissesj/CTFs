# Description
Can you identify the flag hidden within the error messages of this ICMP traffic?

## My solution
I open the pcap file(error_reporting.pcap) with Wireshark and analize it.

It only has only ICMP packets, which is usually used with Ping and traceroute to comunicate informations of network layer(https://pt.wikipedia.org/wiki/Internet_Control_Message_Protocol)

The first packet is a request for a flag and the rest are replies. We can see the beginning of data field in the fist reply that is trasmiting a JFIF file(a compressed form of JPEG to exchange between computers).

We have to extract the data from the source replies and store into a .jpg file to get the flag. To do this, I used scapy library executed in Python3.

```
from scapy.all import *

pcap = rdpcap("error_reporting.pcap")
output = open("flag.jpg", "wb")


for pkt in pcap:
	if pkt[IP].src == '192.168.10.113' and ICMP in pkt:
		output.write(pkt[Raw].load)

output.close() 
```
$ python3 extract3.py
$ open flag.jpg

And there it is


