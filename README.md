# Introduction

Hi There today I published a checklist of strategies on Linux Privilege Escalation by [Tib3rius](https://www.udemy.com/share/101YYoAkoac15TRXo=/)

## Linux Privilege escalation tools :
-	Linux Smart Enumeration (lse.sh)

https://github.com/diego-treitos/linux-smart-enumeration/blob/master/lse.sh

see: ./lse.sh -h

 
-	LinEnum

https://github.com/rebootuser/LinEnum

see: ./LinEnum.sh -h

-	https://github.com/linted/linuxprivchecker
-	https://github.com/AlessandroZ/BeRoot
-	http://pentestmonkey.net/tools/audit/unix-privesc-check







## Privilege Escalation Techniques

1-	Kernel Exploits (last choice) 
  - Ex. searchsploit linux kernel [kernel version] priv esc
  - Ex. searchsploit linux kernel [kernel version] [Linux distribution] priv esc
  - Or use linux exploit suggester 2 tool with argument -k [kernel version]

2-	Services Exploits
  - ps aux | grep "^root" 
  - See the program version to check if its vulnerable <program> --version/-v
  - In Debian-like distributions check "dpkg -l | grep <program>"
  - Systems that use rpm check "rpm -qa | grep <program>" 
  - use "netstat -an" to display the current status of TCP and UDP connections & Remember U can use Port Forwarding if it helps to exploit

3-	Weak File Permissions
  - Writable/Readable /etc/shadow
  - Writable /etc/passwd
  - Backups 
    - Some common locations: / (root) directory, /tmp, and /var/backups

4-	Sudo
  - cat /etc/sudoers
  - sudo -l
  - Known Password
  - https://gtfobins.github.io/
  - Abusing Intended Functionality
  - env_reset 
  - env_keep=LD_PRELOAD
  - env_keep+=LD_Library_Path
    - ldd <program>

5-	Cron Jobs
  - User Cron table files : /var/spool/cron/ or /var/spool/cron/crontabs/
  - Sytem-wide crontab located: /etc/crontab
  - PATH Environment exploit
  - Wildcards & Filenames

6-	SUID / SGID Files
  - **find / -type f -a \( -perm -u+s -o -perm -g+s \) -exec ls -l {} \; 2>/dev/null**
  - if bad interpreter error shown we can solve it with command => **sed -i -e "s/^M//" exploit**
  - strace to see missing shared library
    - **strace [SUID/SGID] 2>&1 | grep -iE "open|access| no such file"**
  - /bin/sh version (notably Bash <4.2-048) export function
  - /bin/sh version (before Bash versions 4.4) SHELLOPTS
  
  
7- Passwords
  - History Files 
  - Confing Files
  - SSH Keys

8-	NFS
  -	no_root_squash

9- Check for services/ports running inside the machine
  - netstat -anop

## Privilege Escalation Strategy

1. Check your user (id, whoami).

2. Run Linux Smart Enumeration with increasing levels.

3. Run LinEnum& other scripts as well!

4. If your scripts are failing and you don’t know why, you can always run the manual commands from this course, and other Linux PrivEsc cheatsheets online (e.g. https://blog.g0tmi1k.com/2011/08/basic-linux-privilege-escalation/) 

5. Spend some time and read over the results of your enumeration

6. If Linux Smart Enumeration level 0 or 1 finds something interesting, make a note of it.

7. Avoid rabbit holes by creating a checklist of things you need for the privilege escalation method to work.

8. Have a quick look around for files in your user’s home directory and other common locations (e.g. /var/backup, /var/logs).If your user has a history file, read it, it may have important information like commands or even passwords.

9. Try things that don’t have many steps first, e.g. Sudo, Cron Jobs, SUID files.

10. Have a good look at root processes, enumerate their versions and search for exploits.

11. Check for internal ports that you might be able to forward to your attacking machine.

12. If you still don’t have root, re-read your full enumeration dumps and highlight anything that seems odd.This might be a process or file name you aren’t familiar with, an “unusual” filesystem configured (on Linux, anything that isn’t ext, swap, or tmpfs), or even a username.At this stage you can also start to think about Kernel Exploits.
