```snmp
snmpwalk -v 2c -c public <IP> 1.3.6.1.2.1.1.5.0
```

```snmp
snmpwalk -v 2c -c private <IP>
```

A tool such as [onesixtyone](https://github.com/trailofbits/onesixtyone) can be used to brute force the community string names using a dictionary file of common community strings such as the `dict.txt` file included in the GitHub repo for the tool.
```snmp
onesixtyone -c dict.txt <IP>
```