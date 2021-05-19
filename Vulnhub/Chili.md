Chili
Description

Goal: Get the root shell i.e.(root@localhost:~#) and then obtain flag under /root).
Hint : "If you ever get stuck, try again with the name of the lab"

## Let's Begin

First we are going to use the commandline utility arp-scan to scan our network to discover the target IP address.

![1](https://user-images.githubusercontent.com/25660910/101018882-0ca3df80-3564-11eb-93c1-16891f46c103.JPG)

Now that we have our target IP (192.168.1.112), we can take the next step, enumeration and scan open ports. To perform the scan we will use the tool nmap.
nmap -A -T4 -p- 192.168.1.112

- A: Turns on the version detection

- T4: Agressive scanning

- -p-: scan all 65535 ports


![2](https://user-images.githubusercontent.com/25660910/101018955-25ac9080-3564-11eb-96f9-a874c7cf1ec7.JPG)

The scan's result as we can see shows us that the ports 21 (ftp) and 80 (HTTP)  are open. Since the port 80 is open we can try to open this IP in our browser.

![Capture](https://user-images.githubusercontent.com/25660910/101019074-52f93e80-3564-11eb-8d70-e7d30af9a0bf.JPG)

But there is nothing that could help us in the web site or in the source code. 

After that we try to bruteforce ftp, with the help of the hint that was given by the author, "If you ever get stuck, try again with the name of the lab" ==> chili.
So we wrote hydra -l chili -P /usr/share/wordlists/rockyou.txt and found the password (a1b2c3d4)! And now we can login as chili.


![4](https://user-images.githubusercontent.com/25660910/101019436-e2065680-3564-11eb-964b-2d3e2753999e.JPG)


After digging through some existing files and their premissions we found that there is a writable directory .nano inside /var/www/html, wich means we can upload a reverse shell


![6](https://user-images.githubusercontent.com/25660910/101020059-cea7bb00-3565-11eb-824a-c63e10df3051.JPG)

We downloaded a shell from https://github.com/pentestmonkey/php-reverse-shell.git.

Inside the ftp server:

cd /var/www/html/.nano/
put shell.php
chmod 777 shell.php

After that, we use the tool netcat to listen the port specified in the shell (4444)

![7](https://user-images.githubusercontent.com/25660910/101020110-e121f480-3565-11eb-902d-d7619407b4c8.JPG)


Then we went to the browser so we could start our shell.


![8](https://user-images.githubusercontent.com/25660910/101020187-03b40d80-3566-11eb-8c48-d58aeacc651c.JPG)

And finally we have our shell!


![9](https://user-images.githubusercontent.com/25660910/101020322-3a8a2380-3566-11eb-951c-c90fae2e7b6d.JPG)


But we obtain our shell without tty, so in order to interact with the system


![10](https://user-images.githubusercontent.com/25660910/101020522-889f2700-3566-11eb-8dd4-70b8478a0ec0.JPG)

Now let's explore what we can do here. First we checked the permissions

![11](https://user-images.githubusercontent.com/25660910/101020694-cb60ff00-3566-11eb-84bd-79c8945542c3.JPG)

We try to check also the permissions of the passwd file and...

![12](https://user-images.githubusercontent.com/25660910/101020852-02371500-3567-11eb-9c42-bc6507c8a8dd.JPG)

And we can write there, which means we can add a user with root access. Lucky us, we have a tool called openssl wich generate a password for us.


![13](https://user-images.githubusercontent.com/25660910/101021004-36aad100-3567-11eb-88cb-afa80b324a6f.JPG)


After we have our encrypted password generated. We used the command echo to write the user "nnn" and the password "melancia121"


![14](https://user-images.githubusercontent.com/25660910/101021286-a15c0c80-3567-11eb-99d5-12ae9c2bc741.JPG)


And here we have it folks, our root access!


![15](https://user-images.githubusercontent.com/25660910/101021601-031c7680-3568-11eb-9ce8-03ed05439f29.JPG)
