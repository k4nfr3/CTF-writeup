The Hidden one.

A scan of the machine shows the result.

![](https://github.com/k4nfr3/CTF-writeup/blob/master/Hackvent%202018/hidden_scan.png)

It shoes a FTP and a Telnet service open which has not yet been part of any day challenge. Interesting.

Let's start with the Telnet.

Ah wouah ... a nice animation shows snow flakes falling and then a Santa saying he has no flag for us ...

![](https://github.com/k4nfr3/CTF-writeup/blob/master/Hackvent%202018/hidden_telnet2.png)

A nice animation show from time to time a letter popping up. 

![](https://github.com/k4nfr3/CTF-writeup/blob/master/Hackvent%202018/hidden_telnet.png)

A WireShark sniff of the traffic / Follow TCP stream produce the following Data stream

![](https://github.com/k4nfr3/CTF-writeup/blob/master/Hackvent%202018/hidden_wireshark.png)

We save the ASCII output to a file.
And after a lot of clean up in NotePad++ removing the special chars and the snowflakes.
we finally get a string "ctrl154n1llu51on"

Now we connect to the FTP server.
After a bit of guessing we enter the username santa and are able to login with that password and retrieve the hidden flag.

![](https://github.com/k4nfr3/CTF-writeup/blob/master/Hackvent%202018/hidden_ftp.png)

Yes, we got the flag 

![](https://github.com/k4nfr3/CTF-writeup/blob/master/Hackvent%202018/hidden_flag.png)

