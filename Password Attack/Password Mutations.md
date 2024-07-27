#### Generating Rule-based Wordlist
```shell
hashcat password.list -r custom.rule --stdout | sort -u > mut_password.list
```
#### Generating Wordlists Using CeWL
```shell
cewl https://www.inlanefreight.com -d 4 -m 6 --lowercase -w inlane.wordlist
```

```shell
wc -l inlane.wordlist
```