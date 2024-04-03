Create a new droplet in digital ocean
Create a new droplet with the custom image and config file.

SSH to your droplet “ssh -i .ssh/do-key root@your_server_ip_address”

Install Vim and Nginx
You should first update the package repository
“sudo pacman -Syu”

Install Vim using the following command:
“sudo -S vim”

Install Nginx using the following commang:
“sudo -S nginx”

Enable the nginx service
“sudo systemctl enable nginx”

Make new directory
“sudo makedir -p /web/html/nginx-2420”

Setup a separate server block
We are going to use sites-enabled and sites-available approach.

To do so, create the following two directories:
“sudo mkdir /etc/nginx/sites-available”
“sudo mkdir /etc/nginx/sites-enabled”

Create a file inside sites-available
/etc/nginx/sites-available/nginx-2420.conf

server {
listen 80;
server_name localhost;
location / {
root /web/html/nginx-2420;
index index.html;
}
error_page 500 502 503 504 /50c.html;
location = /50x.html {
root /web.html/nginx-2420;
}

Append include sites-enabled/*; to the end of the http block:
“sudo vim /etc/nginx/nginx.conf”

Include the following:

http {
include sites-enabled/*;
}

Create a symbolic link to enable the site
“sudo ln -s /etc/nginx/sites-available/nginx-2420.conf /etc/nginx/sites-enabled/nginx-2420.conf”

Reload/restart nginx.service
“sudo systemctl restart nginx.service”