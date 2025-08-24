# Server Setup

## DNS Configuration

1. Log in to your DNS provider or domain registrar.
2. Create an **A record** for your domain pointing to the public IP of your VPS.
   - Example: `demo-agent.microdeets.com` → `203.0.113.10` (replace with your IP).
3. Wait for DNS propagation, which may take a few minutes.

## Nginx Configuration

### Basic HTTP proxy

```nginx
server {
    listen 80;
    server_name demo-agent.microdeets.com;
    location / {
        proxy_pass http://localhost:3001;
        proxy_http_version 1.1;
        proxy_set_header Upgrade $http_upgrade;
        proxy_set_header Connection 'upgrade';
        proxy_set_header Host $host;
        proxy_cache_bypass $http_upgrade;
    }
}
```

### Optional HTTPS with Certbot

1. Install Certbot and the Nginx plugin (`sudo apt install certbot python3-certbot-nginx`).
2. Obtain certificates: `sudo certbot --nginx -d demo-agent.microdeets.com`.
3. Replace the HTTP block or add an HTTPS block:

```nginx
server {
    listen 443 ssl;
    server_name demo-agent.microdeets.com;

    ssl_certificate /etc/letsencrypt/live/demo-agent.microdeets.com/fullchain.pem;
    ssl_certificate_key /etc/letsencrypt/live/demo-agent.microdeets.com/privkey.pem;
    include /etc/letsencrypt/options-ssl-nginx.conf;
    ssl_dhparam /etc/letsencrypt/ssl-dhparams.pem;

    location / {
        proxy_pass http://localhost:3001;
        proxy_http_version 1.1;
        proxy_set_header Upgrade $http_upgrade;
        proxy_set_header Connection 'upgrade';
        proxy_set_header Host $host;
        proxy_cache_bypass $http_upgrade;
    }
}
```

## Apache2 Configuration

### Basic HTTP proxy

1. Enable required modules:
   `sudo a2enmod proxy proxy_http proxy_wstunnel ssl rewrite`
2. Example virtual host:

```apache
<VirtualHost *:80>
    ServerName demo-agent.microdeets.com
    ProxyPass "/" "http://localhost:3001/"
    ProxyPassReverse "/" "http://localhost:3001/"
</VirtualHost>
```

### Optional HTTPS with Certbot

1. Install Certbot and the Apache plugin (`sudo apt install certbot python3-certbot-apache`).
2. Obtain certificates: `sudo certbot --apache -d demo-agent.microdeets.com`.
3. Example SSL virtual host:

```apache
<VirtualHost *:443>
    ServerName demo-agent.microdeets.com
    SSLEngine on
    SSLCertificateFile /etc/letsencrypt/live/demo-agent.microdeets.com/fullchain.pem
    SSLCertificateKeyFile /etc/letsencrypt/live/demo-agent.microdeets.com/privkey.pem
    ProxyPass "/" "http://localhost:3001/"
    ProxyPassReverse "/" "http://localhost:3001/"
</VirtualHost>
```

## Project Directory Setup

1. Create the project directory: `sudo mkdir -p /var/www/livechat-admin`.
2. Upload your project files to `/var/www/livechat-admin`, excluding the `node_modules` folder.
3. Copy the environment template: `cp env.example .env` and edit the values.

## Environment Variables

```
DB_HOST=your_db_host
DB_USER=db_username
DB_PASSWORD=password
DB_NAME=livechat
JWT_SECRET=your_super_secret_key$livechat
PORT=3001
```

## Install Dependencies and Run the Server

```bash
npm install
npm run dev
```

