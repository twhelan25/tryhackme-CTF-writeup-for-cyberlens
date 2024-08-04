
![intro](https://github.com/user-attachments/assets/341efde7-07e4-4e6d-b9c4-11cd22bd43ab)

# tryhackme-CTF-writeup-for-cyberLens

This is a walkthrough for the tryhackme CTF CyberLens. I will not provide any flags or passwords as this is intended to be used as a guide.

# Challenge Description

![description](https://github.com/user-attachments/assets/5e04f23a-a20b-4455-a22d-55edde6b6ff9)

As per the instructions, let's add the IP to our /etc/hosts file:
```bash
sudo echo '10.10.179.83 cyberlens.thm' >> /etc/hosts
```

Scanning/Reconnaissance
First off, let's store the target IP as a variable for easy access.

Command: export ip=xx.xx.xx.xx

Next, let's run an nmap scan on the target IP:

nmap -A -v $ip -D RND:10 -oN nmap.txt
Command break down:

-A: This flag enables aggressive scanning. It combines various scan types (like OS detection, version detection, script scanning, and traceroute) into a single scan.

-v: increases verbosity, providing more detailed output during the scan.

—$ip: provides the target IP we stored as the variable $ip.

-D RND:10 Nmap can send additional packets to confuse network intrusion detection systems (IDS) or hide the true source of the scan by randomly selecting up to 10 decoy IP addresses.

-oN nmap.txt: This option specifies normal output that should be saved to a file named “nmap.txt.

This scan reveals a few open ports that will prove very useful:

![nmap1](https://github.com/user-attachments/assets/1e85694b-46d2-472b-b25c-96d0bef09555)
![nmap2](https://github.com/user-attachments/assets/f4c98a3f-721a-4082-8536-284be0651ac9)

Let's head to web sever on port 80. There is an upload image function. I'm going to run further scans using ffuf, gobuster and nikto for further enumeration:
```bash
ffuf -u http://$ip/FUZZ -w /usr/share/wordlists/dirb/common.txt
```
gobuster dir -u $ip -w=/usr/share/wordlists/seclists/Discovery/Web-Content/raft-medium-words.txt -x php,txt -o buster.txt
```
```bash
nikto -h $ip | tee nikto.txt
```
There are some directories that could prove useful:

![gobuster](https://github.com/user-attachments/assets/cf13212b-f7ce-4698-afdd-28f13257f9fa)

![nikto](https://github.com/user-attachments/assets/40632bb4-ccbc-4c1c-90a7-8752317b2d1d)

In the /js directory there is a js file that contains "fetch("http://localhost:61777/meta":

![extractor js](https://github.com/user-attachments/assets/5077b3c7-30a8-4b63-b402-d138f1e64467)

I ran an nmap scan on port 61777 but it didn't yeild much info.

![nmap61777](https://github.com/user-attachments/assets/c04c9ef6-86e1-4340-8c9a-0ed26484fa04)

I'll go ahead and visit the IP on this port.

We get a Tika 1.17 welcome:

![Tika 1 17](https://github.com/user-attachments/assets/a2d37890-2b70-4e02-b44a-12fa96ff247b)

I did a google search for Tika 1.17 exploit, and checked out this Header Command Injection(Metasploit) exploit. 

![Tika 1 17](https://github.com/user-attachments/assets/86ae25c3-fd8e-42c5-b51c-d0a95d884c61)

I'm going to open up msfconsole and search for this exploit.
