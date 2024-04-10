# Assignment 3 - Part 2

This assignment adds a firewall using UFW and a reverse proxy server to the backend included below.

# Breakdown of updates:

- **Firewall** - adds a firewall using UFW
1. Install ufw package using following command:
``` sudo pacman -S ufw```
2. Enable and start the service with systemctl
``` sudo systemctl enable --now ufw.service ```
3. Check ufw service status using following command:
``` sudo ufw status verbose ```
4. Allow port 8080 using following command.
``` sudo ufw allow 8080 ```

- **Backend** -

### Place backend binary on your system
1. Place hello-server file on you system using sftp.
2. Log in to you sftp server
3. Use following command:
```put <file-location>```
4. Your file will be placed in user home directory.
5. Create a new directory:
```sudo mkdir -p /web/backend```
6. Move binary file(hello-server) to a logical location:
```sudo mv ./hello-server /web/backend/```

### Create a new service file to run the backend

1. Below is the backend.service file:

```plaintext
[Unit]
Description=Hello Server

[Service]
ExecStart=/web/backend/hello-server
Restart=always

[Install]
WantedBy=multi-user.target
```

2. Make systemd aware of the changes using following command.
```sudo systemctl daemon-reload```
3. Enable the service to start on boot using following command.
```sudo systemctl enable backend.service```
4. Start the service using following command.
```sudo systemctl start backend.service```

- **Configure Nginx**

### Edit nginx server block, add reverse proxy to backend -

1. Go to /etc/nginx/sites-available and edit the new backend.conf file using vim.
```sudo vim /etc/nginx/sites-available/backend.conf```
2. Below is the backend.conf content:
```
server {
    listen 80;

    root /web/backend;

    index hello-server;

    server_name <local-host address>;

    location / {
        # Define the reverse proxy settings
        proxy_pass http://127.0.0.1:8080;
        proxy_http_version 1.1;
        proxy_set_header Host $host;
        proxy_set_header X-Real-IP $remote_addr;
        proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
        proxy_set_header X-Forwarded-Proto $scheme;
    }
}
```
3. Restart nginx service
```sudo systemctl restart nginx```

