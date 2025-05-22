# THM-Billing

1. Nmap scan

- nmap -Pn -sC -sV 10.10.147.152
- 22/tcp   open  ssh     OpenSSH 8.4p1 Debian 5+deb11u3 (protocol 2.0)
- 80/tcp   open  http    Apache httpd 2.4.56 ((Debian))
- 3306/tcp open  mysql   MariaDB (unauthorized)
- 5038/tcp open  asterisk Asterisk Call Manager 2.10.6

2. Port Enumeration

ssh/22
- not allowed to bruiteforce
- did not have any usernames yet

http/80
- added ip to etc/host 
- 10.10.147.152 billing.thm
- visit http://billing.thm/mbilling/
- dirb, ffuf to find directories 
- http://billing.thm/mbilling/README.md
- README.md showed that magnusBilling 7.x.x is in use
- found two options
  1. msfconsole - https://www.rapid7.com/db/modules/exploit/linux/http/magnusbilling_unauth_rce_cve_2023_30258/
  2. google - https://github.com/tinashelorenzi/CVE-2023-30258-magnus-billing-v7-exploit
- managed to catch a shell with msfconsole or CVE-2023-30258
- found user flag for magnus - 'find /home -name user.txt and cat /home/magnus/user/txt

msql/3306
-tried to loggin but nothing

unknow/5038
- gave us asterick system

3. Enumeration of shell (asterisk) and Escalate Priviledge
- hostname
- uname -a
- env

sudo -l 
- asterisk had NOPASS priv for fail2ban-client command
- [Fail2Ban 0.11.2 Privilege Escalation / Command Execution - exploit database | Vulners.com](https://vulners.com/packetstorm/PACKETSTORM:189989)
- found root flag 
