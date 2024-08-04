
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
