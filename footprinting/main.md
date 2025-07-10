# Footprinting

## Index
- [home](../index.md)

---

## Domain Information
- [crt.sh](https://crt.sh)
```console
#> curl https://crt.sh/json?q=target.com | jq . > target_certificates.json
```
- [Shodan](https://shodan.io)
```console
#> shodan init <API>
#> for i in $(cat ip_list.txt); do shodan host $i; done
```
- [Domain.glass](https://domain.glass)

### DNS
```console
#> dig any target.com
#> dig any @<DNS> target.com
```

## CLoud Resources
- [GreyHatWarefare](https://buckets.grayhatwarfare.com/)
- We can attempt some Google Dorking
    - intext:<company name> inurl:amazonaws.com 
    - intext:<company name> inurl:blob.core.microsoft.net

## FTP
- It is helpful to experiment with the software
- Some vsftp settings:
    - [vsftpd.conf](https://linux.die.net/man/5/vsftpd.conf)
    - [ftpusers](https://man7.org/linux/man-pages/man5/ftpusers.5.html)

| Setting                       | Description                           |
|:---                           | :---                                  |
| listen=NO                     | standalone mode; default=YES          |
| anonymous_enable=YES          | enable anonymous login                |
| anon_upload_enable=YES        | enable anonymous to upload            |
| anon_mkdir_write_enable=YES   | enable anonymous to create new dirs   |
| no_anon_password=YES          | don't ask anonymous for password      |
| anon_root=/home/username/ftp  | home directory for anonymous          |
| write_enable=YES              | allow multiple FTP commands           |


```console
#> cat /etc/vsftpd.conf
#> wget -m --no-passive ftp://anonymous:anonymous@<FTP> # download all available files
#> nmap -sVC -p21 10.10.10.10
#> openssl s_client -connect 10.10.10.10:21 -starttls ftp # if ftp server uses SSL
```

## SMB
- [MS SMB Docs](https://learn.microsoft.com/en-us/openspecs/windows_protocols/ms-smb/f210069c-7086-4dc2-885e-861d837df688)

### Samba
- [CIFS](https://learn.microsoft.com/en-us/openspecs/windows_protocols/ms-cifs/934c2faa-54af-4526-ac74-6a24d126724e)
- [smb.conf](https://www.samba.org/samba/docs/current/man-html/smb.conf.5.html)

### RPC

| Query                     | Description                                       |
| :---                      | :---                                              |
| srvinfo                   | server info.                                      |
| enumdomains               | enum all domains in network                       |
| querydominfo              | domain, server, & user info on deployed domains   |
| netshareenumall           | enumerate all available shares                    |
| netsharegetinfo <share>   | info on specific share                            |
| enumdomusers              | enumerate all domain users                        |
| queryuser <RID>           | info on specific user                             |

```console
#> smblcient -L -N //10.10.10.10 # list shares; null session
#> smbclient -U "anonymous" //10.10.10.10/<share> # attempt connecting to share
#> rpcclient -U "" 10.10.10.10 # connect to SMB
#> for i in $(seq 500 1000); do rpcclient -N -U "" 10.10.10.10 \ 
   -c "queryuser 0x$(printf '%x\n' $i)" \
   | grep "User Name\|user_rid\|group_rid" && echo ""; done
#> samrdump.py 10.10.10.10
#> smbmap -H 10.10.10.10
#> crackmapexec smb 10.10.10.10 -u '' -p '' --shares
#> enum4linux-ng -A 10.10.10.10
```

## NFS
- Same purpose as SMB
- [NFSv4 RFC](https://datatracker.ietf.org/doc/html/rfc8881)
- No direct mechanism for authentication or authorization.
- Authentication is shifted to RPC
- Authorization is derived from the filesystem
    - server is responsible for translating client info into<br>
      filesystem format and converting auth. details into proper UNIX syntax.
- [/etc/exports](https://manpages.ubuntu.com/manpages/trusty/man5/exports.5.html)

| Options           | Description                                           |
| :---              | :---                                                  |
| rw                | read/write perms                                      |
| ro                | read only perms                                       |
| sync              | synchronous transfers (slower)                        |
| async             | asynchronous transfers (faster)                       |
| secure            | ports above 1024 not used                             |
| insecure          | ports above 1024 will be used                         |
| no_subtree_check  | disable checking of subdir tree                       |
| root_squash       | assign all files of root (UID/GUID 0) to anonymous.<br>Prevents root from accessing NFS mount  |
| no_root_squash    | all files created by root are kept with UID/GUID 0    |
| nohide            | check man exports                                     |

- NFS can provide further priv escalation.
    - If you have access via an ssh user, you can create a shell with SUID<br>
      of another user and place it on the NFS. Then run the shell via ssh user.

```console
#> showmount -e 10.10.10.10
#> mkdir target_nfs
#> sudo mount -t nfs 10.10.10.10:/ ./target_nfs -o nolock
#> cd ./target_nfs
#> sudo umount ./target_nfs
```

---

- [home](../index.md)