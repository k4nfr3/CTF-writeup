A PCAP file was given.

Opening it showed a bunch of ICMP request and replies with different length and payload.
The host was live and responding to ping.

I then tried to get all String out of it. And two lines stood out.
and there seemed to be a string before the PWD like it was a command and then there was the C2C response
![](https://github.com/k4nfr3/CTF-writeup/blob/master/2018/Y-NOT-CTF/writeup1.png)


SCAPY seemed the only way forward.

After a little bit of feedling, and the results of the command HELP.
Here was the final answer to the flag

![](https://github.com/k4nfr3/CTF-writeup/blob/master/2018/Y-NOT-CTF/writeup2.png)




