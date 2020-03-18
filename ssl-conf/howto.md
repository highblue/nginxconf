# Tested on the rasperry pi and domain example name is set as nginxdocs.com

## Configure the SSL

```bash
sudo mkdir /etc/nginx/ssl
sudo chmod 700 /etc/nginx/ssl
sudo openssl req -x509 -nodes -days 365 -newkey rsa:2048 -keyout /etc/nginx/ssl/nginxdocs.key -out /etc/nginx/ssl/nginxdocs.crt
```

## Modify the conf

conf file
```conf
server {
    listen 80 default_server;
    server_name www.nginxdocs.com;
    return 301 https://$server_name$request_uri;

}

server {
    listen 443 ssl;
    server_name www.nginxdocs.com;

    ssl_certificate /etc/nginx/ssl/nginxdocs.crt;
    ssl_certificate_key /etc/nginx/ssl/nginxdocs.key;

    location / {
        root /usr/share/nginx/html;
        index index.html index.htm;
    }
}
```

## Test & Run
```bash
sudo nginx -t
sudo nginx -s reload
```

## curl www.nginxdocs.com
