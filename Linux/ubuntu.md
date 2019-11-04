# UBUNTU CHEETSHEET



RELOAD & RESTART SERVICE

```bash
sudo systemctl reload mysql.service
sudo systemctl restart mysql.service
```

```bash
sudo ufw allow 5432/tcp
```

> Allow port 5432 in uncomplicated firewall



<hr>

#### USER MANAGEMENT

<br>

1. List all users:

```bash
cut -d: -f1 /etc/passwd
```

### 

2. Create new user:

```bash
adduser username
sudo adduser username
```



3. Add user to sudo group:

```bash
usermod -aG sudo username
sudo usermod -aG sudo username
```



4. List all groups

```bash
getent group
```



5. Create new group:

```bash
addgroup groupname
```



6. Delete group:

```bash
groupdel Group_Name
```



7. Add user to group:

```bash
adduser username groupname
```



8. Delete user:

```bash
userdel username
```

### 

9. Modify username:

```bash
usermod -l new_username old_username
```

#### 

10. Set user password:

```bash
passwd username
```



11. Change shell of user:

```bash
chsh username
```

#### 

12. Show detail information of user:

```bash
finger username
id username
```

13. Switch to root user:
```bash
sudo -i
```


<hr>
<br>

#### HOW TO SSH WITHOUT PASSWORD (MORE SECURE):

 <br>

**Client:** 

1. Generate private and public keys:
   - Generate ```id_rsa``` : private key
   - Generate ```id_rsa.pub``` : public key
   - [Optional] : More secure with option ```-b 4096``` 
   - [Optional] : add passphrase 

```bash
ssh-keygen -t rsa -b 4096
```

2. Copy file via ssh to the server:

```bash
scp ~/.ssh/id_rsa.pub username@<host/ip>:/home/username/.ssh/uploaded_key.pub
```

> Note: In this case, Server will need to allow password authentication for accessible
>
> ```bash
> sudo /etc/ssh/sshd_config
> 
> # in sshd_config
> PasswordAuthentication yes
> ```

<br>

**Server:**

In folder home/username:

* Create folder .ssh

```bash
mkdir .ssh
```

* Append key in uploaded_key.pub to authorized_keys file

```bash
cat ~/.ssh/uploaded_key.pub >> ~/.ssh/authorized_keys
chmod 700 ~/.ssh
chmod 600 ~/.ssh/*
```

* Reset ```ssh_config``` file PasswordAuthentication no and restart service

```bash
sudo /etc/ssh/sshd_config

# in sshd_config
PasswordAuthentication yes

# restart
sudo service ssh restart
```



<hr>

SERVICES

```bash
# List all services
service --status-all

sudo service <service-name> restart
### example: sudo service ssh restart
```



