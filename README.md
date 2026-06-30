# Enigma-HTB
nmap report
```bash
└─$ cat nmap.txt          
Starting Nmap 7.99 ( https://nmap.org ) at 2026-06-27 16:06 -0300
Nmap scan report for enigma.htb (10.129.28.213)
Host is up (0.060s latency).
Not shown: 992 closed tcp ports (reset)
PORT     STATE SERVICE  VERSION
22/tcp   open  ssh      OpenSSH 9.6p1 Ubuntu 3ubuntu13.16 (Ubuntu Linux; protocol 2.0)
| ssh-hostkey: 
|   256 0c:4b:d2:76:ab:10:06:92:05:dc:f7:55:94:7f:18:df (ECDSA)
|_  256 2d:6d:4a:4c:ee:2e:11:b6:c8:90:e6:83:e9:df:38:b0 (ED25519)
80/tcp   open  http     nginx 1.24.0 (Ubuntu)
|_http-server-header: nginx/1.24.0 (Ubuntu)
|_http-title: Enigma Corp \xE2\x80\x94 Managed IT Solutions
110/tcp  open  pop3     Dovecot pop3d
|_ssl-date: TLS randomness does not represent time
| ssl-cert: Subject: commonName=enigma
| Subject Alternative Name: DNS:enigma
| Not valid before: 2026-02-18T20:33:33
|_Not valid after:  2036-02-16T20:33:33
|_pop3-capabilities: UIDL SASL PIPELINING RESP-CODES STLS AUTH-RESP-CODE CAPA TOP
111/tcp  open  rpcbind  2-4 (RPC #100000)
| rpcinfo: 
|   program version    port/proto  service
|   100000  2,3,4        111/tcp   rpcbind
|   100000  2,3,4        111/udp   rpcbind
|   100000  3,4          111/tcp6  rpcbind
|   100000  3,4          111/udp6  rpcbind
|   100003  3,4         2049/tcp   nfs
|   100003  3,4         2049/tcp6  nfs
|   100005  1,2,3      32975/tcp6  mountd
|   100005  1,2,3      40895/udp6  mountd
|   100005  1,2,3      41707/tcp   mountd
|   100005  1,2,3      55426/udp   mountd
|   100021  1,3,4      38155/udp6  nlockmgr
|   100021  1,3,4      38707/tcp6  nlockmgr
|   100021  1,3,4      46729/tcp   nlockmgr
|   100021  1,3,4      46918/udp   nlockmgr
|   100024  1          35085/tcp   status
|   100024  1          40826/udp   status
|   100024  1          47149/tcp6  status
|   100024  1          58246/udp6  status
|   100227  3           2049/tcp   nfs_acl
|_  100227  3           2049/tcp6  nfs_acl
143/tcp  open  imap     Dovecot imapd (Ubuntu)
|_imap-capabilities: IMAP4rev1 IDLE SASL-IR have post-login OK ENABLE capabilities LOGINDISABLEDA0001 STARTTLS Pre-login ID listed LOGIN-REFERRALS more LITERAL+
| ssl-cert: Subject: commonName=enigma
| Subject Alternative Name: DNS:enigma
| Not valid before: 2026-02-18T20:33:33
|_Not valid after:  2036-02-16T20:33:33
|_ssl-date: TLS randomness does not represent time
993/tcp  open  ssl/imap Dovecot imapd (Ubuntu)
|_imap-capabilities: IDLE SASL-IR LOGIN-REFERRALS have OK post-login Pre-login AUTH=PLAINA0001 IMAP4rev1 capabilities ID listed ENABLE more LITERAL+
| ssl-cert: Subject: commonName=enigma
| Subject Alternative Name: DNS:enigma
| Not valid before: 2026-02-18T20:33:33
|_Not valid after:  2036-02-16T20:33:33
|_ssl-date: TLS randomness does not represent time
995/tcp  open  ssl/pop3 Dovecot pop3d
|_pop3-capabilities: UIDL SASL(PLAIN) PIPELINING RESP-CODES USER AUTH-RESP-CODE CAPA TOP
| ssl-cert: Subject: commonName=enigma
| Subject Alternative Name: DNS:enigma
| Not valid before: 2026-02-18T20:33:33
|_Not valid after:  2036-02-16T20:33:33
|_ssl-date: TLS randomness does not represent time
2049/tcp open  nfs_acl  3 (RPC #100227)
Device type: general purpose
Running: Linux 5.X
OS CPE: cpe:/o:linux:linux_kernel:5
OS details: Linux 5.0 - 5.14
Network Distance: 2 hops
Service Info: OS: Linux; CPE: cpe:/o:linux:linux_kernel

TRACEROUTE (using port 554/tcp)
HOP RTT       ADDRESS
1   124.03 ms 10.10.16.1
2   41.02 ms  enigma.htb (10.129.28.213)

OS and Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
Nmap done: 1 IP address (1 host up) scanned in 22.69 seconds
```

Many things interesting here, let's start enumeration the NFS server if possible. NFS is a client-server protocol that allow for remote accessing of directories. To see if there are any directories made available publicly use showmount:
```bash
showmount -e enigma.htb
```

The response is:
```bash 
Export list for enigma.htb:
/srv/nfs/onboarding *
```

The asterisk here means that any client can mount the "onboarding" directorie. Of course that includes us.

```bash

