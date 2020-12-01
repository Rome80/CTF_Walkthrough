# Kioptrix - Level 1

## Description

This Kioptrix VM Image are easy challenges. The object of the game is to acquire root access via any means possible (except actually hacking the VM server or player). The purpose of these games are to learn the basic tools and techniques in vulnerability assessment and exploitation. There are more ways then one to successfully complete the challenges.

### Let's Begin

First we are going to use the commandline utility arp-scan to scan our network to discover the target IP address. 


![i](https://user-images.githubusercontent.com/25660910/100739655-4abbca80-33cf-11eb-8eff-44ddf3f34117.jpg)


Now that we have our target IP (192.168.1.100), we can take the next step, enumeration and scan open ports. To perform the scan we will use the tool nmap. 

### nmap -A -T4 -p- 192.168.1.100

-A: Turns on the version detection

-T4: Agressive scanning

-p-: scan all 65535 ports


![Capture](https://user-images.githubusercontent.com/25660910/100740637-f0bc0480-33d0-11eb-9e93-cbb94a2b24e4.JPG)

The scan´s result as we can see shows us that the ports 22 (SSH), 80 (HTTP), 111 (rcpbind), 139 (samba), 443 (https) and 1024 (rcp) are open. Since the port 80 is open we can try to open this IP in our browser.


![Capture](https://user-images.githubusercontent.com/25660910/100742177-45ac4a80-33d2-11eb-9e44-332fb38f3dd5.JPG)

And for the next step we will use Nikto! This tool is a very popular web server scanner that performs comprehensive tests againsts web servers for multiple items that will help us to identify the existing vulnerabilities.

#### nikto -h 192.168.1.100

![Capture](https://user-images.githubusercontent.com/25660910/100743697-915ff380-33d4-11eb-9abe-d090a4625bb6.JPG)

As we can see, several vulnerabilities were identified. But in this process, the vulnerabilities that we want are those that can be explored remotely. Luckly we have one!

#### + mod_ssl/2.8.4 - mod_ssl 2.8.7 and lower are vulnerable to a remote buffer overflow which may allow a remote shell







