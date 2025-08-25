# Server Setup

## DNS Configuration

1. Log in to your DNS provider or domain registrar.
2. Create an **A record** for your domain pointing to the public IP of your VPS.
   - Example: `demo-agent.microdeets.com` → `203.0.113.10` (replace with your IP).
3. Wait for DNS propagation, which may take a few minutes.

## Nginx Configuration

###  HTTPS with SSL

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
## Setup admin user in Environment Variables
```
DEFAULT_ADMIN_EMAIL=admin@example.com
DEFAULT_ADMIN_PASSWORD=change_me
DEFAULT_ADMIN_NAME=Admin
DEFAULT_ADMIN_ROLE=admin
DEFAULT_ADMIN_ENABLED=1
```

## Install Dependencies and Run the Server

```bash
npm install --production
sudo npm install -g pm2
pm2 start npm --name "livechat-admin" -- run start
pm2 startup systemd
pm2 save
pm2 logs livechat-admin
```

## Flutter App Configuration (Client Side)
- Update your Flutter AppConfig to use the new domain:
  ```
class AppConfig {
  static String get baseUrl => 'https://support.microdeets.com';
  static String get apiBaseUrl => '${baseUrl}/api';

  static const String appName = "Microdeets Support Chat";
  static const String appVersion = "1.0.0";

  // Timeout durations
  static const Duration apiTimeout = Duration(seconds: 10);

  // Route paths
  static const String loginRoute = "/login";
  static const String chatListRoute = "/chats";
  static const String chatDetailRoute = "/chat";

  // Shared Preferences keys
  static const String authTokenKey = "jwt_token";
  static const String userInfoKey = "user_info";
}

  ```