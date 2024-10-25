# Overview
Lexactyl is an helictyl based open-source pterodactyl management client area for with a clean good UI made in nodejs

### Features
Resource Management (Use it to create servers, edit them, etc.)

Coins (AFK Page earning)

Servers (create, view, edit servers)

User System (auth, regen password, etc)

OAuth2 (Discord)

Store (buy resources with coins)

Dashboard (view resources & servers)

Admin (set, add, remove coins/scan eggs & locations)


# Sponsors
Ko-fi sponsors are not working so come on our [discord](https://discord.gg/Z24TkRDSUH)

# Installation
You can get started straight away by following these steps:

1. Clone the repo: Run git clone https://github.com/Lexactyl/panel on your machine
2. Enter the directory and configure the config.tomel file - most are optional except the Pterodactyl and OAuth2 settings which must be configured
3. Check everything out and make sure you've configured Lexactyl correctly
4. Create SSL certificates for your target domain and set up the NGINX reverse proxy

# Nginx Proxy Config
```Nginx
server {
    listen 80;
    server_name <domain>;
    return 301 https://$server_name$request_uri;
}

server {
    listen 443 ssl http2;

    location /ws {
      proxy_http_version 1.1;
      proxy_set_header Upgrade $http_upgrade;
      proxy_set_header Connection "upgrade";
      proxy_pass "http://localhost:<port>/ws";
    }

    server_name <domain>;

    ssl_certificate /etc/letsencrypt/live/<domain>/fullchain.pem;
    ssl_certificate_key /etc/letsencrypt/live/<domain>/privkey.pem;
    ssl_session_cache shared:SSL:10m;
    ssl_protocols SSLv3 TLSv1 TLSv1.1 TLSv1.2;
    ssl_ciphers  HIGH:!aNULL:!MD5;
    ssl_prefer_server_ciphers on;

    location / {
      proxy_pass http://localhost:<port>/;
      proxy_buffering off;
      proxy_set_header X-Real-IP $remote_addr;
    }
}

```

Without ssl

```Nginx
server {
    listen 80;
    server_name <domain>;

    location /ws {
      proxy_http_version 1.1;
      proxy_set_header Upgrade $http_upgrade;
      proxy_set_header Connection "upgrade";
      proxy_pass "http://localhost:<port>/ws";
    }

    location / {
      proxy_pass http://localhost:<port>/;
      proxy_buffering off;
      proxy_set_header X-Real-IP $remote_addr;
    }
}
```

# License
(c) 2024 Lynix and contributors. All rights reserved. Licensed under the MIT License.

