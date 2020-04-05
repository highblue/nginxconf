# 1. Configure SSL 


example domain: nginxdocs.com
tested on rasperry pi.

1) Configure the SSL

```bash
sudo mkdir /etc/nginx/ssl
sudo chmod 700 /etc/nginx/ssl
sudo openssl req -x509 -nodes -days 365 -newkey rsa:2048 -keyout /etc/nginx/ssl/nginxdocs.key -out /etc/nginx/ssl/nginxdocs.crt
```

2) Modify the conf

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

3) Test & Run

```bash
sudo nginx -t
sudo nginx -s reload
```

4) Add to the /etc/hosts and curl the domain name.

reference [nginx/youtube.video](https://www.youtube.com/watch?v=X3Pr5VATOyA)

# 2. SSL Certification tools

Server-side encryption

check the link for more info to configure the server side
[mozilla.github.io](https://wiki.mozilla.org/Security/Server_Side_TLS)

# 3. Usful links to exercise

Brad Traversy [Steps to deploy a Node.js app to DigitalOcean using PM2, NGINX as a reverse proxy and an SSL from LetsEncryp](https://gist.github.com/bradtraversy/cd90d1ed3c462fe3bddd11bf8953a896#nodejs-deployment)

# 4. Preferred option - using the certbot

refer to the link https://certbot.eff.org/lets-encrypt/ubuntubionic-nginx
if you have mutliple domains want to activate, please consider using this
//Please specify --domains, or --installer that will help in domain names autodiscovery, or --cert-name for an existing certificate name.

example:
sudo certbot --nginx --domains www.example.com

# 5. Check your cert

if you have successfully publish your website with https, check with this link provided in certbo.eff.org
https://www.ssllabs.com/ssltest/

# 6. Useful links to configure the SSL automatically

probably the best tool to configure the SSL and related conf with UI - The easiest way to configure a performant, secure,
and stable NGINX server from digitalocean.com https://www.digitalocean.com/community/tools/nginx 
or 
you can build your own <https://github.com/digitalocean/nginxconfig.io>

