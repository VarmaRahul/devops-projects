## Prerequisites
- Get a domain name from domain registrar and SSL certificate from provider.
- Keep the certificate bundle and private key saved for further use.

### Setting up the server and installing Nginx
- Start with launching a ubuntu machine with minimal config
- Create a key-pair for ssh authentication
- Enable port for ssh(22), http(80) and https(443) in the security group
- Login to the machine via ssh using console/putty, update os and install nginx
```
sudo apt-get update && sudo apt-get -y upgrade
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
git clone https://github.com/VarmaRahul/Hospital-Management-Html.git
mv Hospital-Management-Html hospital
chmod 666 hospital
```

### Setting up Nginx for web routing
- Go to the nginx config file and setup routing for the hospital home page
- The public ip should be redirected to the index page in the project folder
```
cd /etc/nginx/sites-available/
chmod 666 default
vim default
```
- Replace the file with below content
- The server will listen on port 80 on your domain
```
server {
        listen 80 default_server;
        listen [::]:80 default_server;

        root /var/www/html/hospital/;

        index index.html

        server_name <example.com> <www.example.com>;

        location / {
                # First attempt to serve request as file, then
                # as directory, then fall back to displaying a 404.
                try_files  / =404;
        }
}
```
- restart the nginx service
```
sudo systemctl restart nginx
```
- Go to the Domain registrar, create an A record and add the Public IP as the value.
- Create a CName record for subdomain www.example.com
- The website is now available online, it is still not secured

### Configure SSL on the server
- Download the SSL csr file and private key from the provider
- Browse to location /etc/ssl
- Combine the content of ca_bundle_file + crt file to create domain.crt file
- Copy the private_key to create domain.key file

- Make changes to the default file as below
```
cat >> default <<EOF

server {
    listen 80;
    listen [::]:80;

    server_name example.com www.example.com;

    return 302 https://$server_name$request_uri;
}


server {
	listen 443 ssl;
	listen [::]:443 ssl;
    ssl_certificate /etc/ssl/example.crt
    ssl_certificate_key /etc/ssl/example.key

    server_name example.com www.example.com;

    root /var/www/html/hospital;
    index index.html index.htm index.nginx-debian.html;

    location / {
		# First attempt to serve request as file, then
		# as directory, then fall back to displaying a 404.
		try_files  / =404;
	}
}

EOF
```
- Check if there are any syntax errors in the config
```
sudo nginx -t
```
- Now restart the nginx service
```
sudo systemctl restart nginx
```
- Browse the website to see secured key in the header which shows "Connection is secure"
- Now you website is live on example.com



![image](https://github.com/VarmaRahul/devops-projects/assets/66249367/a2280c6a-56fa-4558-9f49-b2ca828d631d)
