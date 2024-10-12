# EC2 Node.js Deployment

This guide provides step-by-step instructions for deploying a Node.js application to Amazon EC2 using PM2, NGINX as a reverse proxy, and SSL from Let's Encrypt.

## 1. Create Free AWS Account

Create a free AWS Account at [https://aws.amazon.com/](https://aws.amazon.com/)

## 2. Create and Launch an EC2 Instance

For this demo, we'll be creating a t2.medium Ubuntu machine. After launching the instance, SSH into the machine.

## 3. Install Node.js and NPM

Run the following commands to install Node.js and NPM:

```bash
curl -sL https://deb.nodesource.com/setup_18.x | sudo -E bash -
sudo apt install nodejs
node --version
```

## 4. Clone Your Project from GitHub

Clone your project repository:

```bash
git clone https://github.com/piyushgargdev-01/short-url-nodejs
```

## 5. Install Dependencies and Test the App

Install PM2 globally and start your application:

```bash
sudo npm i pm2 -g
pm2 start index
```

Other useful PM2 commands:

```bash
pm2 show app
pm2 status
pm2 restart app
pm2 stop app
pm2 logs         # Show log stream
pm2 flush        # Clear logs

# To ensure the app starts on reboot
pm2 startup ubuntu
```

## 6. Install and Configure NGINX

Install NGINX:

```bash
sudo apt install nginx
```

Configure NGINX:

```bash
sudo nano /etc/nginx/sites-available/default
```

Add the following to the location part of the server block:

```nginx
server_name yourdomain.com www.yourdomain.com;
location / {
    proxy_pass http://localhost:8001; #whatever port your app runs on
    proxy_http_version 1.1;
    proxy_set_header Upgrade $http_upgrade;
    proxy_set_header Connection 'upgrade';
    proxy_set_header Host $host;
    proxy_cache_bypass $http_upgrade;
}
```

Check the NGINX configuration and restart:

```bash
sudo nginx -t
sudo nginx -s reload
```

## 7. Add SSL with Let's Encrypt

Install Certbot and obtain SSL certificates:

```bash
sudo add-apt-repository ppa:certbot/certbot
sudo apt-get update
sudo apt-get install python3-certbot-nginx
sudo certbot --nginx -d yourdomain.com -d www.yourdomain.com
```

Test the renewal process:

```bash
certbot renew --dry-run
```

* SSL certificates are valid for 90 days. Make sure to set up automatic renewal.

---

With these steps, you should have successfully deployed your Node.js application to Amazon EC2 with NGINX as a reverse proxy and SSL from Let's Encrypt.

Credits - https://github.com/piyushgarg-dev
