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



1. List all users:

    cut -d: -f1 /etc/passwd

### 

2. Create new user:

    adduser username
    sudo adduser username



3. Add user to sudo group:

    usermod -aG sudo username
    sudo usermod -aG sudo username



4. List all groups

    getent group



5. Create new group:

    addgroup groupname



6. Delete group:

    groupdel Group_Name



7. Add user to group:

    adduser username groupname



8. Delete user:

    userdel username

### 

9. Modify username:

    usermod -l new_username old_username

#### 

10. Set user password:

    passwd username



11. Change shell of user:

    chsh username

#### 

12. Show detail information of user:

    finger username
    id username


