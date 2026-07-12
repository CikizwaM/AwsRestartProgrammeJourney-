# Managing Log Files

In this lab, I reviewed the **lastlog** and **secure log** outputs of the Linux machine.

## Task 1: Use SSH to connect to an Amazon Linux EC2 instance
1. I connected to the EC2 istance via `SSH`.

## Task 2: Review secure log files
1. To review the **secure log** outputs I entered `sudo less /tmp/log/secure` on the terminal.
```bash
Aug 23 03:47:13 centos7 sshd[3283]: Invalid user guest from 193.201.224.218
Aug 23 03:47:13 centos7 sshd[3283]: input_userauth_request: invalid user guest [preauth]
Aug 23 03:47:13 centos7 sshd[3283]: pam_unix(sshd:auth): check pass; user unknown
Aug 23 03:47:13 centos7 sshd[3283]: pam_unix(sshd:auth): authentication failure; logname= uid=0 euid=0 tty=ssh ruser= rhost=193.201.224.218
Aug 23 03:47:15 centos7 sshd[3283]: Failed password for invalid user guest from 193.201.224.218 port 13181 ssh2
Aug 23 03:47:16 centos7 sshd[3283]: pam_unix(sshd:auth): check pass; user unknown
Aug 23 03:47:17 centos7 sshd[3283]: Failed password for invalid user guest from 193.201.224.218 port 13181 ssh2
Aug 23 03:47:18 centos7 sshd[3283]: pam_unix(sshd:auth): check pass; user unknown
Aug 23 03:47:20 centos7 sshd[3283]: Failed password for invalid user guest from 193.201.224.218 port 13181 ssh2
Aug 23 03:47:24 centos7 sshd[3283]: pam_unix(sshd:auth): check pass; user unknown
Aug 23 03:47:25 centos7 sshd[3283]: Failed password for invalid user guest from 193.201.224.218 port 13181 ssh2
Aug 23 03:47:26 centos7 sshd[3283]: pam_unix(sshd:auth): check pass; user unknown
Aug 23 03:47:27 centos7 sshd[3283]: Failed password for invalid user guest from 193.201.224.218 port 13181 ssh2
Aug 23 03:47:27 centos7 sshd[3283]: pam_unix(sshd:auth): check pass; user unknown
Aug 23 03:47:29 centos7 sshd[3283]: Failed password for invalid user guest from 193.201.224.218 port 13181 ssh2
Aug 23 03:47:29 centos7 sshd[3283]: Disconnecting: Too many authentication failures for guest [preauth]
Aug 23 03:47:13 centos7 sshd[3283]: Invalid user guest from 193.201.224.218
Aug 23 03:52:40 centos7 sshd[5160]: Invalid user acc from 193.201.224.218
Aug 23 03:52:45 centos7 sshd[5243]: Invalid user adam from 193.201.224.218
Aug 23 03:52:53 centos7 sshd[5312]: Invalid user adfexc from 193.201.224.218
Aug 23 03:53:45 centos7 sshd[5494]: Invalid user admin2 from 193.201.224.218
Aug 23 03:53:45 centos7 sshd[5494]: pam_unix(sudo:session): session opened for user root by (uid=0)
Aug 23 03:53:45 centos7 sshd[5494]: pam_succeed_if(sudo:session): 'uid' resolves to '0'
Aug 23 03:53:45 centos7 sshd[5494]: pam_succeed_if(sudo:session): 'user' resolves to 'root'
Aug 23 03:53:45 centos7 sshd[5494]: pam_succeed_if(sudo:session): 'ruser' resolves to 'telegraf'
Aug 23 03:53:45 centos7 sshd[5494]: pam_unix(sudo:session): session closed for user root
Aug 23 05:08:09 centos7 sshd[5185]: pam_succeed_if(sshd:auth): requirement "uid >= 1000" not met by user "root"
Aug 23 05:08:10 centos7 sshd[5185]: Failed password for root from 218.65.30.123 port 42034 ssh2
Aug 23 05:08:11 centos7 sshd[5185]: pam_succeed_if(sshd:auth): requirement "uid >= 1000" not met by user "root"
Aug 23 05:08:13 centos7 sshd[5185]: Failed password for root from 218.65.30.123 port 42034 ssh2
Aug 23 05:08:14 centos7 sshd[5185]: pam_succeed_if(sshd:auth): requirement "uid >= 1000" not met by user "root"
Aug 23 05:08:16 centos7 sshd[5185]: Failed password for root from 218.65.30.123 port 42034 ssh2
Aug 23 05:08:16 centos7 sshd[5185]: pam_succeed_if(sshd:auth): requirement "uid >= 1000" not met by user "root"
Aug 23 05:08:18 centos7 sshd[5185]: Failed password for root from 218.65.30.123 port 42034 ssh2
Aug 23 05:08:19 centos7 sshd[5185]: pam_succeed_if(sshd:auth): requirement "uid >= 1000" not met by user "root"
Aug 23 05:08:21 centos7 sshd[5185]: Failed password for root from 218.65.30.123 port 42034 ssh2
Aug 23 05:08:22 centos7 sshd[5185]: pam_succeed_if(sshd:auth): requirement "uid >= 1000" not met by user "root"
Aug 23 05:08:24 centos7 sshd[5185]: Failed password for root from 218.65.30.123 port 42034 ssh2
/tmp/log/secure
```

2. To view the **last login** times of all the users on the machine, I entered `sudo lastlog` on the terminal.
```bash
[ec2-user@ip-10-0-10-4 companyA]$ sudo lastlog
Username         Port     From             Latest
root                                       **Never logged in**
bin                                        **Never logged in**
daemon                                     **Never logged in**
adm                                        **Never logged in**
lp                                         **Never logged in**
sync                                       **Never logged in**
shutdown                                   **Never logged in**
halt                                       **Never logged in**
mail                                       **Never logged in**
operator                                   **Never logged in**
games                                      **Never logged in**
ftp                                        **Never logged in**
nobody                                     **Never logged in**
systemd-network                            **Never logged in**
dbus                                       **Never logged in**
rpc                                        **Never logged in**
libstoragemgmt                             **Never logged in**
sshd                                       **Never logged in**
rpcuser                                    **Never logged in**
nfsnobody                                  **Never logged in**
ec2-instance-connect                           **Never logged in**
postfix                                    **Never logged in**
rngd                                       **Never logged in**
chrony                                     **Never logged in**
tcpdump                                    **Never logged in**
ec2-user         pts/0    102.221.201.1    Sun Jul 12 22:55:57 +0000 2026
ljuan                                      **Never logged in**
mmajor                                     **Never logged in**
mjackson                                   **Never logged in**
eowusu                                     **Never logged in**
nwolf                                      **Never logged in**
arosalez                                   **Never logged in**
jdoe                                       **Never logged in**
psantos                                    **Never logged in**
smartinez                                  **Never logged in**
ssarkar                                    **Never logged in**
```
## Conclusion
What information can you extract for some of your business purposes?

Except the ec2-user, no other user managed to login to the EC2 istance.
