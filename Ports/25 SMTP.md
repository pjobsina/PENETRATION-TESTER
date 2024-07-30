```shell
telnet 10.129.14.128 25
```

| **Command**  | **Description**                                                                                  |
| ------------ | ------------------------------------------------------------------------------------------------ |
| `AUTH PLAIN` | AUTH is a service extension used to authenticate the client.                                     |
| `HELO`       | The client logs in with its computer name and thus starts the session.                           |
| `MAIL FROM`  | The client names the email sender.                                                               |
| `RCPT TO`    | The client names the email recipient.                                                            |
| `DATA`       | The client initiates the transmission of the email.                                              |
| `RSET`       | The client aborts the initiated transmission but keeps the connection between client and server. |
| `VRFY`       | The client checks if a mailbox is available for message transfer.                                |
| `EXPN`       | The client also checks if a mailbox is available for messaging with this command.                |
| `NOOP`       | The client requests a response from the server to prevent disconnection due to time-out.         |
| `QUIT`       | The client terminates the session.                                                               |
#### Default Configuration
```shell
cat /etc/postfix/main.cf | grep -v "#" | sed -r "/^\s*$/d"
```
#### Enumeration
```shell
sudo nmap 10.129.14.128 -sC -sV -p25
```
```shell
sudo nmap 10.129.14.128 -p25 --script smtp-open-relay -v
```
[smtp-open-relay](https://nmap.org/nsedoc/scripts/smtp-open-relay.html) NSE script to identify the target SMTP server as an open relay using 16 different tests.
#### Telnet - HELO/EHLO
```shell
HELO mail1.inlanefreight.htb

EHLO mail1
```
#### Telnet - VRFY
```shell
VRFY root

VRFY <user>

VRFY testuser

VRFY aaaaaaaaaaaaaaaaaaaaaaaaaaaa
```
#### Send an Email
```shell
EHLO inlanefreight.htb

MAIL FROM: <cry0l1t3@inlanefreight.htb>

250 2.1.0 Ok


RCPT TO: <mrb3n@inlanefreight.htb> NOTIFY=success,failure

250 2.1.5 Ok


DATA

354 End data with <CR><LF>.<CR><LF>

From: <cry0l1t3@inlanefreight.htb>
To: <mrb3n@inlanefreight.htb>
Subject: DB
Date: Tue, 28 Sept 2021 16:32:51 +0200
Hey man, I am trying to access our XY-DB but the creds don't work. 
Did you make any changes there?
.

250 2.0.0 Ok: queued as 6E1CF1681AB


QUIT
```
#### IMAP COMMANDS:
https://donsutherland.org/crib/imap
https://www.atmail.com/blog/imap-101-manual-imap-sessions/
https://www.mailenable.com/kb/content/article.asp?ID=ME020711

A1 LOGIN "username" "password"
A1 LIST "" *
A1 SELECT INBOX
A2 FETCH 1 BODY[]
F FETCH 1:1 (BODY[HEADER.FIELDS (Subject)])
F FETCH 1 RFC822
