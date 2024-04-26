### This requires following the static website config
- Create a new ubuntu server and execute the below commands
```
sudo apt-get update && sudo apt-get -y upgrade
sudo apt-get install nginx -y
sudo apt-get install git -y
```
- Change the file /etc/nginx/sites-available/default with the below config
- The proxy_pass is where the redirecting server name/IP needs to be specified
```
server {
    listen 80;
    listen [::]:80;

    server_name _;

    location / {
        proxy_pass https://example.com;
        proxy_set_header Host $host;
        proxy_set_header X-Real-IP $remote_addr;
        proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
        proxy_set_header X-Forwarded-Proto $scheme;
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
- Browse the IP of newly created server and observe the reverse proxy rerouting to your domain
