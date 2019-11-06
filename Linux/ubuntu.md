# UBUNTU

<br><br>

## USER MANAGEMENT

```bash
adduser <username> : # add new user
userdel <username> : # delete user
passwd <username> : # set password
usermod -l <new_username> <old_username> : # update username
usermod -aG sudo <username> : # add user sudo group

getent group : # list all groups
addgroup <groupname> : # add new group
groupdel <groupname> : # delete group
adduser <username> <groupname>

chsh <username> : # change shell user
id <username> : # show user info
sudo -i : # switch to root user
su - <username> : # switch user
cut -d: -f1 /etc/passwd : # list all users
```

<br><br>

## SERVICE MANAGEMENT

```bash
# start/stop service
systemctl start application.service
systemctl stop application.service

# manage running service
systemctl restart application.service
systemctl reload application.service
systemctl reload-or-restart application.service

# start/stop services automatically at boot
systemctl enable application.service
systemctl disable application.service

# check service
systemctl status application.service
systemctl is-active application.service
systemctl is-enabled application.service
systemctl is-failed application.service
systemctl list-units --type:service : # list units type service that systemd currently has active on the system

systemctl poweroff : # shutdown
systemctl reboot : # restart
sudo reboot : # reboot the system
```

<br><br>

## UFW (UNCOMPLICATED FIREWALL)

```bash
ufw allow <port>/tcp : # allow port 
ufw app list : # list available application 
ufw allow 'Nginx HTTP' : # allow 'Nginx HTTP'
ufw status : # list active application
```

<br><br>

## NETWORKING

```bash
ifconfig
# check IP address assigned to the system

telnet <domain-name/IP> <port> 
# telnet connect destination host:port via a telnet protocol if connection establishes means connectivity between two hosts is working fine

nslookup <domain-name/IP>
# nslookup is a program to query Internet domain name servers => return name and IP

netstat
# command allows you a simple way to review each of your network connections and open sockets

scp $filename user@targethost:/$path
# scp allows you to secure copy files to and from another host in the network

w 
# w prints a summary of the current activity on the system, including what each user is doing, and their processes. Also list the logged in users and system load average for the past 1, 5, and 15 minutes
```

<br><br>

<hr>

## SET UP SSH WITHOUT PASSWORD (MORE SECURE):

**CLIENT:** 

1. Generate private and public keys:
   - Generate ```id_rsa``` : private key
   - Generate ```id_rsa.pub``` : public key
   - [Optional] : More secure with option ```-b 4096``` 
   - [Optional] : add passphrase 

```bash
ssh-keygen -t rsa -b 4096
```

3. Copy file via ssh to the server:

```bash
scp ~/.ssh/id_rsa.pub <username>@<host/ip>:/home/username/.ssh/uploaded_key.pub
```

> Note: Allow password authentication to be able to upload the file to server
>
> ```bash
> # edit /etc/ssh/sshd_config
> PasswordAuthentication yes
> ```

**SERVER:**

2. Create folder .ssh

```bash
mkdir /home/username/.ssh
```

4. Append key in uploaded_key.pub to authorized_keys file

```bash
cat ~/.ssh/uploaded_key.pub >> ~/.ssh/authorized_keys
chmod 700 ~/.ssh
chmod 600 ~/.ssh/*
```

* Reset ```sshd_config``` file PasswordAuthentication no and restart service

```bash
# edit /etc/ssh/sshd_config
PasswordAuthentication no

# restart
sudo service ssh restart
```

<br><br>

## POSTGRESQL

**INSTALLATION:**

```bash
# 1. Enable postgresql apt repository
apt-get install wget ca-certificates
wget --quiet -O - https://www.postgresql.org/media/keys/ACCC4CF8.asc | sudo apt-key add -

# 2. Add repository to your system
sh -c 'echo "deb http://apt.postgresql.org/pub/repos/apt/ `lsb_release -cs`-pgdg main" >> /etc/apt/sources.list.d/pgdg.list'

# 3. Install postgresql
apt-get update
apt-get install postgresql postgresql-contrib

# 4. Switch to postgres user
su - postgres

# 5. Access psql to set password for postgres user
psql
postgres=# \password
postgres=# \conninfo
postgres=# \q

exit
```



**ENABLE REMOTE CONNECTION**

1. Edit: pg_hba.conf

```bash
# location: /etc/postgresql/<version>/main/pg_hba.conf

# IPv4 local connections:
host    all             all             0.0.0.0/0               md5
```

> - 0.0.0.0/0 : to allow all IPs accessible
> - md5 : to access with username and password



2. Edit: postgresql.conf

```bash
# location: /etc/postgresql/<version>/main/postgresql.conf

# - Connection Settings -
listen_addresses = '*'          # what IP address(es) to listen on;
```

<br><br>

## NGINX

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

<br>

**RENEW SSL**

```bash
# Only valid for 90 days, test the renewal process with
certbot renew --dry-run
```

