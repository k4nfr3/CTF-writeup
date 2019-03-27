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

Hint : We got the password "SyN" at 1am

Ok, let's send some commands
main(SyN ls -la)
....
final command : main(SyN cat ./secret/me/not/flag.txt)

to find flag 
![](https://github.com/k4nfr3/CTF-writeup/blob/master/2019-Insomnihack/Net1.jpg)



# mybrokenbash

nc server 1337    
Let's send the command **cat flag** but we get cat flag in return  
ok so maybe stdout is closed  
but **cat flag 2>&1** doesn't work either    
so maybe stderr is also closed  
let's redirect stdout to stdin  
**cat flag 1>&0**    
We are on the good track but we are missing something still.  
The file had a carriage return (\r), and only the end of the file was sent back  
One had to find/know that cat has options.  
** cat -A** = show-all
Final command to show the flag was ** cat -A flag 1>&0**

# Skeleton in the closet

We download the dump file  
![](https://github.com/k4nfr3/CTF-writeup/blob/master/2019-Insomnihack/skeleton1.jpg)

![](https://github.com/k4nfr3/CTF-writeup/blob/master/2019-Insomnihack/skeleton2.jpg)

![](https://github.com/k4nfr3/CTF-writeup/blob/master/2019-Insomnihack/skeleton2b.jpg)

![](https://github.com/k4nfr3/CTF-writeup/blob/master/2019-Insomnihack/skeleton2c.jpg)

![](https://github.com/k4nfr3/CTF-writeup/blob/master/2019-Insomnihack/skeleton3.jpg)

![](https://github.com/k4nfr3/CTF-writeup/blob/master/2019-Insomnihack/skeleton4.jpg)





