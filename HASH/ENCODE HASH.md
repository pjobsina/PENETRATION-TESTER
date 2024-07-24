# ==**BASE64**==
**Encode**
```
echo https://www.hackthebox.eu/ | base64
```
**Decode**
```
echo aHR0cHM6Ly93d3cuaGFja3RoZWJveC5ldS8K | base64 -d
```
# **HEX**
**Encode**
```
echo https://www.hackthebox.eu/ | xxd -p
```
**Decode**
```
echo 68747470733a2f2f7777772e6861636b746865626f782e65752f0a | xxd -p -r
```
# **ROT13** 
**Encode**
```
echo https://www.hackthebox.eu/ | tr 'A-Za-z' 'N-ZA-Mn-za-m'
```
**Decode**
```
echo uggcf://jjj.unpxgurobk.rh/ | tr 'A-Za-z' 'N-ZA-Mn-za-m'
```
# **CYPHER**
```
https://www.boxentriq.com/code-breaking/cipher-identifier
```
