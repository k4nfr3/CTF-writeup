Knockknock
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

it is supposed to happen between port TCP3000-4000  
a quick : nmap -Pn -p 3000-4000 10.13.37.99  
![](https://github.com/k4nfr3/CTF-writeup/blob/master/2019-Insomnihack/Net00.jpg)


Hint 2 : RFC741x => RFC7413 = Use TCP SYN Data  

ok, Let's try some Python Scapy with TCP-Syn Data  

![](https://github.com/k4nfr3/CTF-writeup/blob/master/2019-Insomnihack/Net0.jpg)

Trying to send some commands but nothing back from the server.  
The goal was to send starting with just one letter.  
When receiving the wrong password, no answer. But sending a truncated password : for example Sy, the server would have returned with an answer of 'rx truncated' in the SYN-ACK packet.  

Hint got released at 1am : Password was "SyN"  

Ok, let's send some commands  
main(SyN ls -la)  
....  
final command : main(SyN cat ./secret/me/not/flag.txt)  


![](https://github.com/k4nfr3/CTF-writeup/blob/master/2019-Insomnihack/Net1.jpg)




