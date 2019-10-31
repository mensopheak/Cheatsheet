# UBUNTU CHEETSHEET





```bash
sudo systemctl reload mysql.service
sudo systemctl restart mysql.service
```





```bash
sudo ufw allow 5432/tcp
```

> Allow port 5432 in uncomplicated firewall

## User Management


### List all users

    cut -d: -f1 /etc/passwd

### Create SUDO user

#### Create username

    adduser username
    sudo adduser username

#### Add user into SUDO group

    usermod -aG sudo username
    sudo usermod -aG sudo username

### List all groups

    getent group

### Create Group

    addgroup groupname

### Delete Group

    groupdel Group_Name

### Add user to created group

    adduser username groupname

### Delete user

    userdel username

### Modify user

#### To modify the username of a user:

    usermod -l new_username old_username

#### To change the password for a user:

    passwd username

#### To change the shell for a user:

    chsh username

#### To show detail user information:

    finger username
    id username


