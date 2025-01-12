TP3 Sécu : Linux Hardening


Sommaire : 
```
0. Setup
1. Guides CIS
2. Conf SSH
4. DoT
5. AIDE
```

1. Guides CIS

🌞 Suivre un guide CIS
### 2.1.1
```
# rpm -q chrony
chrony-4.2-1.el8.rocky.1.0.x86_64
```
### 2.1.2
```
# grep -E "^(server|pool)" /etc/chrony.conf
pool 2.rocky.pool.ntp.org iburst

# grep ^OPTIONS /etc/sysconfig/chronyd
OPTIONS="-u chrony"
```
## 3.1 
### 3.1.1
```
# grep -Pqs '^\h*0\b' /sys/module/ipv6/parameters/disable && echo -e "\n -
> IPv6 is enabled\n" || echo -e "\n - IPv6 is not enabled\n"

 -
IPv6 is enabled
```
### 3.1.2
```
# bash wire.sh 

- Audit Result:
 ** PASS **

 - System has no wireless NICs installed
```
### 3.1.3
```
 # bash tipc.sh 
- Audit Result:
 ** PASS **

 - Module "tipc" doesn't exist on the
system
```
## 3.2
### 3.2.1
```
# bash ipforward.sh 
- Audit Result:
 ** FAIL **
 - Reason(s) for audit
failure:

 - "net.ipv4.ip_forward = 0" is not set
in a kernel parameter configuration file
 - "net.ipv6.conf.all.forwarding = 0" is not set
in a kernel parameter configuration file

- Correctly set:

 - "net.ipv4.ip_forward" is set to
"0" in the running configuration
 - "net.ipv4.ip_forward" is not set incorectly in
a kernel parameter configuration file
 - "net.ipv6.conf.all.forwarding" is set to
"0" in the running configuration
 - "net.ipv6.conf.all.forwarding" is not set incorectly in
a kernel parameter configuration file

# printf "
net.ipv4.ip_forward = 0
" >> /etc/sysctl.d/60-netipv4_sysctl.conf


# {
sysctl -w net.ipv4.ip_forward=0
sysctl -w net.ipv4.route.flush=1
}

net.ipv4.ip_forward = 0
net.ipv4.route.flush = 1

# printf "
net.ipv6.conf.all.forwarding = 0
" >> /etc/sysctl.d/60-netipv6_sysctl.conf

# {
 sysctl -w net.ipv6.conf.all.forwarding=0
 sysctl -w net.ipv6.route.flush=1
 }
 
net.ipv6.conf.all.forwarding = 0
net.ipv6.route.flush = 1
```
### 3.2.2
```
# bash ipforward.sh 
- Audit Result:
 ** PASS **

 - "net.ipv4.ip_forward" is set to
"0" in the running configuration
 - "net.ipv4.ip_forward" is set to "0"
in "/etc/sysctl.d/60-netipv4_sysctl.conf"
 - "net.ipv4.ip_forward" is not set incorectly in
a kernel parameter configuration file
 - "net.ipv6.conf.all.forwarding" is set to
"0" in the running configuration
 - "net.ipv6.conf.all.forwarding" is set to "0"
in "/etc/sysctl.d/60-netipv6_sysctl.conf"
 - "net.ipv6.conf.all.forwarding" is not set incorectly in
a kernel parameter configuration file

# printf "
 net.ipv4.conf.all.send_redirects = 0
 net.ipv4.conf.default.send_redirects = 0
 " >> /etc/sysctl.d/60-netipv4_sysctl.conf

# {
 sysctl -w net.ipv4.conf.all.send_redirects=0
 sysctl -w net.ipv4.conf.default.send_redirects=0
 sysctl -w net.ipv4.route.flush=1
 }

net.ipv4.conf.all.send_redirects = 0
net.ipv4.conf.default.send_redirects = 0
net.ipv4.route.flush = 1
```  
## 3.3
### 3.3.1
```
# cat /etc/sysctl.d/60-netipv4_sysctl.conf 

net.ipv4.ip_forward = 0

net.ipv4.conf.all.send_redirects = 0
net.ipv4.conf.default.send_redirects = 0

printf "
net.ipv4.conf.all.accept_source_route = 0
net.ipv4.conf.default.accept_source_route = 0
" >> /etc/sysctl.d/60-netipv4_sysctl.conf

# {
> sysctl -w net.ipv4.conf.all.accept_source_route=0
> sysctl -w net.ipv4.conf.default.accept_source_route=0
> sysctl -w net.ipv4.route.flush=1
> }
net.ipv4.conf.all.accept_source_route = 0
net.ipv4.conf.default.accept_source_route = 0
net.ipv4.route.flush = 1


# cat /etc/sysctl.d/60-netipv6_sysctl.conf 

net.ipv6.conf.all.forwarding = 0


# printf "
> net.ipv6.conf.all.accept_source_route = 0
> net.ipv6.conf.default.accept_source_route = 0
> " >> /etc/sysctl.d/60-netipv6_sysctl.conf
# {
> sysctl -w net.ipv6.conf.all.accept_source_route=0
> sysctl -w net.ipv6.conf.default.accept_source_route=0
> sysctl -w net.ipv6.route.flush=1
> }
net.ipv6.conf.all.accept_source_route = 0
net.ipv6.conf.default.accept_source_route = 0
net.ipv6.route.flush = 1



# cat /etc/sysctl.d/60-netipv4_sysctl.conf 

net.ipv4.ip_forward = 0

net.ipv4.conf.all.send_redirects = 0
net.ipv4.conf.default.send_redirects = 0

net.ipv4.conf.all.accept_source_route = 0
net.ipv4.conf.default.accept_source_route = 0
# printf "
> net.ipv4.conf.all.accept_redirects = 0
> net.ipv4.conf.default.accept_redirects = 0
> " >> /etc/sysctl.d/60-netipv4_sysctl.conf
# {
> sysctl -w net.ipv4.conf.all.accept_redirects=0
> sysctl -w net.ipv4.conf.default.accept_redirects=0
> sysctl -w net.ipv4.route.flush=1
> }
net.ipv4.conf.all.accept_redirects = 0
net.ipv4.conf.default.accept_redirects = 0
net.ipv4.route.flush = 1
# cat /etc/sysctl.d/60-netipv6_sysctl.conf                                       

net.ipv6.conf.all.forwarding = 0

net.ipv6.conf.all.accept_source_route = 0
net.ipv6.conf.default.accept_source_route = 0
# printf "
> net.ipv6.conf.all.accept_redirects = 0
> net.ipv6.conf.default.accept_redirects = 0
> " >> /etc/sysctl.d/60-netipv6_sysctl.conf
# {
> sysctl -w net.ipv6.conf.all.accept_redirects=0
> sysctl -w net.ipv6.conf.default.accept_redirects=0
> sysctl -w net.ipv6.route.flush=1
> }
net.ipv6.conf.all.accept_redirects = 0
net.ipv6.conf.default.accept_redirects = 0
net.ipv6.route.flush = 1

3.3.3

# cat /etc/sysctl.d/60-netipv4_sysctl.conf 

net.ipv4.ip_forward = 0

net.ipv4.conf.all.send_redirects = 0
net.ipv4.conf.default.send_redirects = 0

net.ipv4.conf.all.accept_source_route = 0
net.ipv4.conf.default.accept_source_route = 0

net.ipv4.conf.all.accept_redirects = 0
net.ipv4.conf.default.accept_redirects = 0
# printf "
> net.ipv4.conf.all.secure_redirects = 0
> net.ipv4.conf.default.secure_redirects = 0
> " >> /etc/sysctl.d/60-netipv4_sysctl.conf
# {
> sysctl -w net.ipv4.conf.all.secure_redirects=0
> sysctl -w net.ipv4.conf.default.secure_redirects=0
> sysctl -w net.ipv4.route.flush=1
> }
net.ipv4.conf.all.secure_redirects = 0
net.ipv4.conf.default.secure_redirects = 0
net.ipv4.route.flush = 1

3.3.4


# cat /etc/sysctl.d/60-netipv4_sysctl.conf

net.ipv4.ip_forward = 0

net.ipv4.conf.all.send_redirects = 0
net.ipv4.conf.default.send_redirects = 0

net.ipv4.conf.all.accept_source_route = 0
net.ipv4.conf.default.accept_source_route = 0

net.ipv4.conf.all.accept_redirects = 0
net.ipv4.conf.default.accept_redirects = 0

net.ipv4.conf.all.secure_redirects = 0
net.ipv4.conf.default.secure_redirects = 0
# printf "
> net.ipv4.conf.all.log_martians = 1
> net.ipv4.conf.default.log_martians = 1
> " >> /etc/sysctl.d/60-netipv4_sysctl.conf
# {
> sysctl -w net.ipv4.conf.all.log_martians=1
> sysctl -w net.ipv4.conf.default.log_martians=1
> sysctl -w net.ipv4.route.flush=1
> }
net.ipv4.conf.all.log_martians = 1
net.ipv4.conf.default.log_martians = 1
net.ipv4.route.flush = 1


3.3.5

# cat /etc/sysctl.d/60-netipv4_sysctl.conf

net.ipv4.ip_forward = 0

net.ipv4.conf.all.send_redirects = 0
net.ipv4.conf.default.send_redirects = 0

net.ipv4.conf.all.accept_source_route = 0
net.ipv4.conf.default.accept_source_route = 0

net.ipv4.conf.all.accept_redirects = 0
net.ipv4.conf.default.accept_redirects = 0

net.ipv4.conf.all.secure_redirects = 0
net.ipv4.conf.default.secure_redirects = 0

net.ipv4.conf.all.log_martians = 1
net.ipv4.conf.default.log_martians = 1
# printf "
> net.ipv4.icmp_echo_ignore_broadcasts = 1
> " >> /etc/sysctl.d/60-netipv4_sysctl.conf
# {
> sysctl -w net.ipv4.icmp_echo_ignore_broadcasts=1
> sysctl -w net.ipv4.route.flush=1
> }
net.ipv4.icmp_echo_ignore_broadcasts = 1
net.ipv4.route.flush = 1


3.3.6

# cat /etc/sysctl.d/60-netipv4_sysctl.conf

net.ipv4.ip_forward = 0

net.ipv4.conf.all.send_redirects = 0
net.ipv4.conf.default.send_redirects = 0

net.ipv4.conf.all.accept_source_route = 0
net.ipv4.conf.default.accept_source_route = 0

net.ipv4.conf.all.accept_redirects = 0
net.ipv4.conf.default.accept_redirects = 0

net.ipv4.conf.all.secure_redirects = 0
net.ipv4.conf.default.secure_redirects = 0

net.ipv4.conf.all.log_martians = 1
net.ipv4.conf.default.log_martians = 1

net.ipv4.icmp_echo_ignore_broadcasts = 1
# printf "
> net.ipv4.icmp_ignore_bogus_error_responses = 1
> " >> /etc/sysctl.d/60-netipv4_sysctl.conf
# {
> sysctl -w net.ipv4.icmp_ignore_bogus_error_responses=1
> sysctl -w net.ipv4.route.flush=1
> }
net.ipv4.icmp_ignore_bogus_error_responses = 1
net.ipv4.route.flush = 1

3.3.7

# cat /etc/sysctl.d/60-netipv4_sysctl.conf                                           
net.ipv4.ip_forward = 0

net.ipv4.conf.all.send_redirects = 0
net.ipv4.conf.default.send_redirects = 0

net.ipv4.conf.all.accept_source_route = 0
net.ipv4.conf.default.accept_source_route = 0

net.ipv4.conf.all.accept_redirects = 0
net.ipv4.conf.default.accept_redirects = 0

net.ipv4.conf.all.secure_redirects = 0
net.ipv4.conf.default.secure_redirects = 0

net.ipv4.conf.all.log_martians = 1
net.ipv4.conf.default.log_martians = 1

net.ipv4.icmp_echo_ignore_broadcasts = 1

net.ipv4.icmp_ignore_bogus_error_responses = 1
# printf "
> net.ipv4.conf.all.rp_filter = 1
> net.ipv4.conf.default.rp_filter = 1
> " >> /etc/sysctl.d/60-netipv4_sysctl.conf
# {
> sysctl -w net.ipv4.conf.all.rp_filter=1
> sysctl -w net.ipv4.conf.default.rp_filter=1
> sysctl -w net.ipv4.route.flush=1
> }
net.ipv4.conf.all.rp_filter = 1
net.ipv4.conf.default.rp_filter = 1
net.ipv4.route.flush = 1

3.3.8

# cat /etc/sysctl.d/60-netipv4_sysctl.conf

net.ipv4.ip_forward = 0

net.ipv4.conf.all.send_redirects = 0
net.ipv4.conf.default.send_redirects = 0

net.ipv4.conf.all.accept_source_route = 0
net.ipv4.conf.default.accept_source_route = 0

net.ipv4.conf.all.accept_redirects = 0
net.ipv4.conf.default.accept_redirects = 0

net.ipv4.conf.all.secure_redirects = 0
net.ipv4.conf.default.secure_redirects = 0

net.ipv4.conf.all.log_martians = 1
net.ipv4.conf.default.log_martians = 1

net.ipv4.icmp_echo_ignore_broadcasts = 1

net.ipv4.icmp_ignore_bogus_error_responses = 1

net.ipv4.conf.all.rp_filter = 1
net.ipv4.conf.default.rp_filter = 1
# printf "
> net.ipv4.tcp_syncookies = 1
> " >> /etc/sysctl.d/60-netipv4_sysctl.conf
# {
> sysctl -w net.ipv4.tcp_syncookies=1
> sysctl -w net.ipv4.route.flush=1
> }
net.ipv4.tcp_syncookies = 1
net.ipv4.route.flush = 1


3.3.9

# cat /etc/sysctl.d/60-netipv6_sysctl.conf

net.ipv6.conf.all.forwarding = 0

net.ipv6.conf.all.accept_source_route = 0
net.ipv6.conf.default.accept_source_route = 0

net.ipv6.conf.all.accept_redirects = 0
net.ipv6.conf.default.accept_redirects = 0
# printf "
> net.ipv6.conf.all.accept_ra = 0
> net.ipv6.conf.default.accept_ra = 0
> " >> /etc/sysctl.d/60-netipv6_sysctl.conf
# {
> sysctl -w net.ipv6.conf.all.accept_ra=0
> sysctl -w net.ipv6.conf.default.accept_ra=0
> sysctl -w net.ipv6.route.flush=1
> }
net.ipv6.conf.all.accept_ra = 0
net.ipv6.conf.default.accept_ra = 0
net.ipv6.route.flush = 1

```
## 5.2
```
5.2.1
# stat -Lc "%n %a %u/%U %g/%G" /etc/ssh/sshd_config
/etc/ssh/sshd_config 600 0/root 0/root


5.2.2

# chmod 0600 /etc/ssh/ssh_host_ecdsa_key
# chmod 0600 /etc/ssh/ssh_host_ed25519_key
# chmod 0600 /etc/ssh/ssh_host_rsa_key

5.2.3

# bash ssh.sh 

- Audit Result:
 - Correctly
set:

 - Public key file: "/etc/ssh/ssh_host_ed25519_key.pub" is mode
"0644" should be mode: "644" or more restrictive
 - Public key file: "/etc/ssh/ssh_host_ecdsa_key.pub" is mode
"0644" should be mode: "644" or more restrictive
 - Public key file: "/etc/ssh/ssh_host_rsa_key.pub" is mode
"0644" should be mode: "644" or more restrictive

# ls -al /etc/ssh/
total 616
drwxr-xr-x.  3 root root       4096  6 janv. 09:14 .
drwxr-xr-x. 87 root root       8192 12 janv. 03:06 ..
-rw-r--r--.  1 root root     577388 11 août 12:35 moduli
-rw-r--r--.  1 root root       1770 11 août 12:35 ssh_config
drwxr-xr-x.  2 root root         28 28 déc.  03:04 ssh_config.d
-rw-------.  1 root root       4267  6 janv. 09:14 sshd_config
-rw-------.  1 root ssh_keys    480 28 déc.  04:07 ssh_host_ecdsa_key
-rw-r--r--.  1 root root        162 28 déc.  04:07 ssh_host_ecdsa_key.pub
-rw-------.  1 root ssh_keys    387 28 déc.  04:07 ssh_host_ed25519_key
-rw-r--r--.  1 root root         82 28 déc.  04:07 ssh_host_ed25519_key.pub
-rw-------.  1 root ssh_keys   2578 28 déc.  04:07 ssh_host_rsa_key
-rw-r--r--.  1 root root        554 28 déc.  04:07 ssh_host_rsa_key.pub

5.2.4

# cat /etc/ssh/sshd_config | grep AllowUsers
AllowUsers user

5.2.5

# cat /etc/ssh/sshd_config | grep LogLevel
LogLevel INFO

5.2.6

# cat /etc/ssh/sshd_config | grep UsePAM
UsePAM yes

5.2.7

# cat /etc/ssh/sshd_config | grep PermitRootLogin
PermitRootLogin no

5.2.8

# cat /etc/ssh/sshd_config | grep Hostbased
HostbasedAuthentication no

5.2.9

# cat /etc/ssh/sshd_config | grep PermitEmpty
PermitEmptyPasswords no


5.2.10

# cat /etc/ssh/sshd_config | grep PermitUser
PermitUserEnvironment no

5.2.11

# cat /etc/ssh/sshd_config | grep IgnoreR
IgnoreRhosts yes

5.2.12

# cat /etc/ssh/sshd_config | grep X11For
X11Forwarding no

5.2.13

# cat /etc/ssh/sshd_config | grep AllowT
AllowTcpForwarding no

5.2.14

# grep -i '^\s*CRYPTO_POLICY=' /etc/sysconfig/sshd

5.2.15

# cat /etc/ssh/sshd_config | grep Banner
Banner /etc/issue.net

5.2.16

# cat /etc/ssh/sshd_config | grep MaxAuth
MaxAuthTries 4

5.2.17

# cat /etc/ssh/sshd_config | grep MaxStart
MaxStartups 10:30:60

5.2.18

# cat /etc/ssh/sshd_config | grep MaxSes
MaxSessions 10

5.2.19

# cat /etc/ssh/sshd_config | grep LoginGrace
LoginGraceTime 60

5.2.20

# cat /etc/ssh/sshd_config | grep Client
ClientAliveInterval 15
ClientAliveCountMax 3

```
  - au moins 10 points dans la section 6.1 System File Permissions

```
6.1.1

# stat -Lc "%n %a %u/%U %g/%G" /etc/passwd
/etc/passwd 644 0/root 0/root

6.1.2

# stat -Lc "%n %a %u/%U %g/%G" /etc/passwd-
/etc/passwd- 644 0/root 0/root

6.1.3

# stat -Lc "%n %a %u/%U %g/%G" /etc/group
/etc/group 644 0/root 0/root

6.1.4

# stat -Lc "%n %a %u/%U %g/%G" /etc/group-
/etc/group- 644 0/root 0/root

6.1.5

# stat -Lc "%n %a %u/%U %g/%G" /etc/shadow
/etc/shadow 0 0/root 0/root

6.1.6

# stat -Lc "%n %a %u/%U %g/%G" /etc/shadow-
/etc/shadow- 0 0/root 0/root

6.1.7

# stat -Lc "%n %a %u/%U %g/%G" /etc/gshadow
/etc/gshadow 0 0/root 0/root

6.1.8

# stat -Lc "%n %a %u/%U %g/%G" /etc/gshadow-
/etc/gshadow- 0 0/root 0/root

6.1.9

# df --local -P | awk '{if (NR!=1) print $6}' | xargs -I '{}' find '{}' -xdev -type f -perm -0002

6.1.10

# df --local -P | awk {'if (NR!=1) print $6'} | xargs -I '{}' find '{}' -xdev -nouser

6.1.11

# df --local -P | awk '{if (NR!=1) print $6}' | xargs -I '{}' find '{}' -xdev -nogroup

```
  - au moins 10 points ailleur sur un truc que vous trouvez utile

```
5.3.3

# cat /etc/sudoers | grep logfile
Defaults logfile="/var/log/sudo.log"

5.3.4

# grep -r "^[^#].*NOPASSWD" /etc/sudoers*

5.3.5

# grep -r "^[^#].*\!authenticate" /etc/sudoers*

5.3.6

# cat /etc/sudoers | grep time
Defaults    timestamp_timeout=5

1.5.1

# cat /etc/systemd/coredump.conf | grep Storage
Storage=none

2.3.1

# rpm -q telnet
package telnet is not installed

2.3.2

# rpm -q openldap-clients
package openldap-clients is not installed

2.3.3

# rpm -q tftp
package tftp is not installed

2.3.4

# rpm -q ftp
package ftp is not installed

5.6.1.1

# grep PASS_MAX_DAYS /etc/login.defs
PASS_MAX_DAYS	365

```
🌞 **Chiffrement fort côté serveur**

```
$ sudo rm /etc/ssh/ssh_host_*
$ sudo rm ~/.ssh/id_*
$ echo "ssh-ed25519 AAAAC3NzaC1lZDI1NTE5AAAAILx3fqeQSeFfcJFsQx1E8BC8aCi/fWoK6M+fuKB/lOZ0 BATPC" | sudo tee /home/$USER/.ssh/authorized_keys
$ ssh-keygen -o -a 256 -t ed25519
$ sudo systemctl sshd restart
```
[Conf ssh](sshd_config)

🌞 **Clés de chiffrement fortes pour le client**

```
BATPC:~$ ssh-keygen -o -a 256 -t ed25519
```

🌞 **Connectez-vous en SSH à votre VM avec cette paire de clés**

[Preuve connexion ssh](connexion_ssh)

## 4. DoT

Ca commence à faire quelques années maintenant que plusieurs acteurs poussent pour qu'on fasse du DNS chiffré, et qu'on arrête d'envoyer des requêtes DNS en clair dans tous les sens.

Le Dot est une techno qui va dans ce sens : DoT pour DNS over TLS. On fait nos requêtes DNS dans des tunnels chiffrés avec le protocole TLS.

🌞 **Configurer la machine pour qu'elle fasse du DoT**

```
$ dnf install epel-release -y
$ dnf update -y
$ dnf install systemd-resolved -y
$ systemctl enable systemd-resolved
$ systemctl start systemd-resolved

$ cat /etc/systemd/resolved.conf 
...
[Resolve]
DNS=1.1.1.1
Domains=~
DNSSEC=yes
DNSOverTLS=yes
...

$ systemctl restart systemd-resolved

$ ln -sf /run/systemd/resolve/stub-resolv.conf /etc/resolv.conf
```

🌞 **Prouvez que les requêtes DNS effectuées par la machine...**

[Pcap dns over tls](DNS_Over_TLS.pcap)

```
$ tcpdump -v -i enp0s8 port 853
dropped privs to tcpdump
tcpdump: listening on enp0s8, link-type EN10MB (Ethernet), snapshot length 262144 bytes
11:16:41.405498 IP (tos 0x0, ttl 64, id 51393, offset 0, flags [DF], proto TCP (6), length 64)tp3.44446 > one.one.one.one.domain-s: Flags [S], cksum 0x0f43 (incorrect -> 0x15a9), seq 2249304843, win 64240, options [mss 1460,sackOK,TS val 1357496916 ecr 0,nop,wscale 7,tfo  cookiereq,nop,nop], length 0
```

## 5. AIDE

🌞 **Installer et configurer AIDE**

```
$ dnf install aide
```
- configurez AIDE pour qu'il surveille (fichier de conf en compte-rendu)
  - le fichier de conf du serveur SSH
```
$  cat /etc/aide.conf | grep ssh
# ssh
/etc/ssh/sshd_config$ CONTENT_EX
/etc/ssh/ssh_config$ CONTENT_EX
```
  - le fichier de conf du client chrony (le service qui gère le temps)
```
$ cat /etc/aide.conf | grep chrony
/etc/chrony.conf$ CONTENT_EX
/etc/chrony.keys$ CONTENT_EX
```
  - le fichier de conf de `systemd-networkd`
```
$ cat /etc/aide.conf | grep systemd-networkd
/etc/systemd/resolved.conf$ CONTENT_EX
/etc/resolv.conf$ CONTENT_EX
```

🌞 **Scénario de modification**

- introduisez une modification dans le fichier de conf du serveur SSH
```
$ echo "#miao" >> /etc/ssh/sshd_config 
```
- montrez que AIDE peut la détecter

```
$ aide --check
Start timestamp: 2025-01-12 04:47:53 -0500 (AIDE 0.16)
AIDE found differences between database and filesystem!!

Summary:
  Total number of entries:	46980
  Added entries:		0
  Removed entries:		0
  Changed entries:		1

---------------------------------------------------
Changed entries:
---------------------------------------------------

f   ...    .C... : /etc/ssh/sshd_config

---------------------------------------------------
Detailed information about changes:
---------------------------------------------------

File: /etc/ssh/sshd_config
  SHA512   : M5jreLePw8iPODBcNfhuoCo91SMaeRPT | rfitfAzqxORWzo2Dnw+GG5uFUhW6HT4N
             wFHXfC741C/xnyJNCcrv74cBxrUoWqHc | aRiWqU9NJvC5Rr6/dGolGd++QayFlhiI
             avC+uIBMDWjuiIcMNb5V3Q==         | mqQ5FmUX69Cxopl+FeRxYg==
```