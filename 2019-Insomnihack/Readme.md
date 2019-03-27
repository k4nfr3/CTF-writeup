# Knockknock

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

# Net1

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

After searching a bit, we found the reference of the Skeleton Misc function of the well known Gentil Kiwi Mimikatz.
https://github.com/gentilkiwi/mimikatz  
We find from the documentation, this function will patch the LSASS process and adds a super password which will work for any username used !   
By default, the password is : **mimikatz**  
The NTLM hash of mimikatz is :  
![](https://github.com/k4nfr3/CTF-writeup/blob/master/2019-Insomnihack/skeleton0b.jpg)  

We can see from the source code of mimikatz this NTML password in little indian format.  

![](https://github.com/k4nfr3/CTF-writeup/blob/master/2019-Insomnihack/skeleton0.jpg)  

The definition of the chall, is that a personalized version of mimikatz has been used. 
So we need to find the password which has been encoded in his customized version.  

We download the memory dump file and analyse it with Volatility.   

**python vol.py -f memory.vmem imageinfo.** To find the OS.  
![](https://github.com/k4nfr3/CTF-writeup/blob/master/2019-Insomnihack/skeleton1.jpg)

**python vol.py -f memory.vmem --profile=Win2016x64_14393 pslist.** To find the process running.    
![](https://github.com/k4nfr3/CTF-writeup/blob/master/2019-Insomnihack/skeleton2.jpg)

**python vol.py -f memory.vmem --profile=Win2016x64_14393 -p 588 memdump -D ./dump.**
![](https://github.com/k4nfr3/CTF-writeup/blob/master/2019-Insomnihack/skeleton2b.jpg)

Now we need to find how this alteration of this skeleton function has left traces in the memory.  
To find this, we use a standard Domain Controller and apply the function with the original version of Mimikatz.  
**mimikatz**  
**privilege::debug**    
**misc::skeleton**  

![](https://github.com/k4nfr3/CTF-writeup/blob/master/2019-Insomnihack/skeleton2bb.jpg)

After dumping the memory of the lsass process, let's analyse how the original mimikatz has writen the NTLM hash in the memory.  
We can use many tools, but here we use **hexedit lsass_post.DMP**  
And search for out hash starting with **60ba4f**  (Ctrl-S)
We can see the NTLM hash with a structure around it **C7 44 24 xx** between each part.  
(In blue the hash, orange the repeated pattern).    
![](https://github.com/k4nfr3/CTF-writeup/blob/master/2019-Insomnihack/skeleton3.jpg)

Great, let's do the same on the dumped memory from Volatility and search for this pattern **C7 44 24**  

![](https://github.com/k4nfr3/CTF-writeup/blob/master/2019-Insomnihack/skeleton4.jpg)

The NTLM hash to crack is 65FB3480 14C7B62A 100AF97E EC0B6221  
And we know from the chall definition that it is **INS{xxxxxxx}** (7 chars to find)  

Let's bruteforce the hash:  
John:**john --format=nt -mask=INS{?1?1?1?1?1?1?1} -1=?l?u?d ./myhash.txt**  
Hashcat: **hashcat64 --session skeleton hash.txt -m 1000 -a 3 INS{?1?1?1?1?1?1?1} -1 ?l?u?d**  



