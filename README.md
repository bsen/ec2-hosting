# EC2 Node.js Deployment Guide

## 1. AWS Setup
- Create AWS account
- Launch EC2 instance
- SSH into machine

## 2. Node.js Setup
```bash
curl -o- https://raw.githubusercontent.com/nvm-sh/nvm/v0.39.3/install.sh | bash

. ~/.nvm/nvm.sh

nvm install 23
```

## 3. Project Setup
```bash
git clone https://github.com/your-username/your-repository

cd your-repository

npm install

sudo npm i pm2 -g

pm2 start index.js
```

## 4. NGINX Setup
```bash
sudo apt update

sudo apt install nginx

sudo nano /etc/nginx/sites-available/default
```

Add to server block:
```nginx
server {
    server_name yourdomain.com www.yourdomain.com;

    location / {
        proxy_pass http://localhost:8001;
        proxy_http_version 1.1;
        proxy_set_header Upgrade $http_upgrade;
        proxy_set_header Connection 'upgrade';
        proxy_set_header Host $host;
        proxy_cache_bypass $http_upgrade;
    }
}
```

Then verify and restart NGINX:
```bash
sudo nginx -t

sudo systemctl restart nginx

sudo nginx -s reload
```

## 5. SSL Setup
```bash
sudo apt-get update

sudo apt-get install python3-certbot-nginx

sudo certbot --nginx -d yourdomain.com -d www.yourdomain.com

sudo certbot renew --dry-run
```
