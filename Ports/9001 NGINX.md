## Nginx - Enabling PUT
#### Create a Directory to Handle Uploaded Files
```shell
sudo mkdir -p /var/www/uploads/SecretUploadDirectory
```
#### Change the Owner to www-data
```shell
sudo chown -R www-data:www-data /var/www/uploads/SecretUploadDirectory
```
#### Create Nginx Configuration File
Create the Nginx configuration file by creating the file `/etc/nginx/sites-available/upload.conf` with the contents:
```shell
server {
    listen 9001;
    
    location /SecretUploadDirectory/ {
        root    /var/www/uploads;
        dav_methods PUT;
    }
}
```
#### Symlink our Site to the sites-enabled Directory
```shell
sudo ln -s /etc/nginx/sites-available/upload.conf /etc/nginx/sites-enabled/
```
#### Start Nginx
```shell
sudo systemctl restart nginx.service
```
#### Verifying Errors
```shell
tail -2 /var/log/nginx/error.log
```
```shell
ss -lnpt | grep 80
```
```shell
ps -ef | grep 2811
```
#### Remove NginxDefault Configuration
```shell
sudo rm /etc/nginx/sites-enabled/default
```
#### Upload File Using cURL
```shell
curl -T /etc/passwd http://localhost:9001/SecretUploadDirectory/users.txt
```
```shell
sudo tail -1 /var/www/uploads/SecretUploadDirectory/users.txt 
```