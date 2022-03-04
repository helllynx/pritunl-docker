## Pritunl

How to get password:

```
docker exec -it pritunl sh -c "pritunl default-password"
```

nginx config example:

```
server {
    listen 443 ssl;
    listen [::]:443 ssl;
    server_name pritunl.zybc.pro;

    ssl_certificate /etc/letsencrypt/live/pritunl.zybc.pro/fullchain.pem;
    ssl_certificate_key /etc/letsencrypt/live/pritunl.zybc.pro/privkey.pem;

    ssl_session_cache shared:SSL:1m;
    ssl_session_timeout  5m;
    ssl_ciphers  HIGH:!aNULL:!MD5;
    ssl_prefer_server_ciphers   on;

  location / {
    proxy_pass       http://localhost:9700;
                     proxy_http_version 1.1;
                     proxy_set_header Upgrade $http_upgrade;
                     proxy_set_header Connection "upgrade";
                     proxy_set_header Host $http_host;
                     proxy_set_header X-Real-IP $remote_addr;
                     proxy_set_header X-Forward-For $proxy_add_x_forwarded_for;
                     proxy_set_header X-Forward-Proto http;
                     proxy_set_header X-Nginx-Proxy true;
                     proxy_redirect off;
  }
}
```