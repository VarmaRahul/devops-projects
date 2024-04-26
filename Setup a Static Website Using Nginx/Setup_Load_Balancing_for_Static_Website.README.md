### The prerequisite is to setup two static websites on different Nginx servers- Create a new ubuntu server and execute the below commands
```
sudo apt-get update && sudo apt-get -y upgrade
sudo apt-get install nginx -y
```
- Change the file /etc/nginx/sites-available/default with the below config
- The upstream is where you specify the server details for the load balancing
```
upstream backend {
	server example2.com; 
	server example1.com;  
}

server {
        listen 80;
        listen [::]:80;

        server_name _;

        root /var/www/html/;
        index index.html index.htm index.nginx-debian.html;

        location / {
                # First attempt to serve request as file, then
                # as directory, then fall back to displaying a 404.
                proxy_pass http://backend;
        }
}
```

- Check if there are any syntax errors in the config
```
sudo nginx -t
```
- Now restart the nginx service
```
sudo systemctl restart nginx
```
- Browse the IP of load server multiple times and observe the site getting redirected to different backend servers.
