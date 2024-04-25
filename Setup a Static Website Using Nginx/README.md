### Prerequisites
- Get a domain name from domain registrar and a SSL certificate

### Setting up the server and installing Nginx
- Start with launching a ubuntu machine with minimal config
- Create a key-pair for ssh authentication
- Enable port for ssh(22), http(80) and https(443) in the security group
- Login to the machine via ssh using console/putty, update os and install nginx
```
sudo apt-get update -y
sudo apt-get upgrade -y
sudo apt-get install nginx -y
sudo apt-get install git -y
```
- Verify the default page in the below location
```
sudo cat /var/www/html/index.nginx-debian.html
```
- Browse the public ip of the ubuntu machine opening page
- clone the sample hospital management static website to the /var/www/html folder
- rename the project to "hospital"
```
cd /var/www/html
git clone 
mv Hospital-Management-Html hospital
```
- Go to the nginx config file and setup routing for the hospital home page
- The public ip should be redirected to the index page in the project folder
```
vim /etc/nginx/sites-available/default
```
- Replace the file with the below content
- The server will listen on port 80 on your domain
```
server {
        listen 80 default_server;
        listen [::]:80 default_server;

        root /var/www/html/hospital/;

        index index.html

        server_name <domain_name>;

        location / {
                # First attempt to serve request as file, then
                # as directory, then fall back to displaying a 404.
                try_files  / =404;
        }
}
```
