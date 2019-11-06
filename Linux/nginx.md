# NGINX

HELPER SOURCES:

* https://www.digitalocean.com/community/tutorials/how-to-install-nginx-on-ubuntu-18-04
* [How to setup a firewall with ufw on ubuntu 18-04](https://www.digitalocean.com/community/tutorials/how-to-set-up-a-firewall-with-ufw-on-ubuntu-18-04)

<hr>

```bash
nginx -t : # test nginx
```

<br>

**INSTALLATION**

```bash
apt update
apt install nginx : # install nginx
systemctl enable nginx : # start nginx automatically on boot
systemctl status nginx : # check nginx status
```

NOTICE 1: SITES-AVAILABLE

```bash
/etc/nginx/sites-available
```

NOTICE 2: SITES-ENABLED

```bash
/etc/nginx/sites-enabled
```

<br>

**SIMPLE NGINX CONFIGURATION**

```bash
# 1. configure sites-available/example.com
touch /etc/nginx/sites-available/example.com
nano /etc/nginx/sites-available/example.com
server {
  listen 80;
  listen [::]:80;
  root /var/www/example.com;
  index index.html;
  server_name example.com www.example.com;
  location / {
  	try_files $uri $uri/ =404;
  }
}

# 2. link sites-available to sites-enabled (publish state)
ln -s /etc/nginx/sites-available/example.com /etc/nginx/sites-enabled


# 3. restart nginx
systemctl restart nginx
```



**CONFIGURATION WITH SSL**

```bash
# replace example.com with your_domain.com

server
{ 
  listen 80;
  listen [::]:80;
  server_name example.com;
  return 301 https://$server_name$request_uri;
}
server
{
  listen 443 ssl http2;
  listen [::]:443 ssl;
  server_name example.com;
  ssl on;
  ssl_certificate /etc/letsencrypt/live/example.com/fullchain.pem;
  ssl_certificate_key /etc/letsencrypt/live/example.com/privkey.pem;
  access_log /var/log/nginx/access.log;
  error_log /var/log/nginx/error.log;
  location /.well-known
  {
    root /var/www/example.com;
  }
  
  location /
  {
    proxy_pass http://127.0.0.1:7777/;
    proxy_set_header Host $host;
    proxy_set_header X-Real-IP $remote_addr;
    proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
    proxy_set_header X-Forwarded-Proto $scheme;
  }
}
```

<br>

**INSTALL CERTBOT**

```bash
add-apt-repository ppa:certbot/certbot
apt install python-certbot-nginx
```

<br>

**OBTAIN SSL CERTIFICATE**

```bash
certbot --nginx -d example.com -d www.example.com
```

```bash
1: No redirect - Make no further changes to the webserver configuration.
2: Redirect - Make all requests redirect to secure HTTPS access. Choose this for
new sites, or if you're confident your site works on HTTPS. You can undo this
change by editing your web server's configuration.
- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -
Select the appropriate number [1-2] then [enter] (press 'c' to cancel): 2
Redirecting all traffic on port 80 to ssl in /etc/nginx/sites-enabled/default
Redirecting all traffic on port 80 to ssl in /etc/nginx/sites-enabled/default
```

<br>

**RENEW SSL**

```bash
# Only valid for 90 days, test the renewal process with
certbot renew --dry-run
```

