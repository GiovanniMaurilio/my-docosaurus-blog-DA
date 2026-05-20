# V-Server Setup Guide

This guide explains how to set up a V-Server.  
You will configure SSH access, improve server security, install Nginx, connect GitHub, and manage multiple SSH identities.

---

# Table of Contents

- [1. Create SSH access for the V-Server](#1-create-ssh-access-for-the-v-server)
- [2. Install and start a web server](#2-install-and-start-a-web-server)
- [3. Change the Nginx default page](#3-change-the-nginx-default-page)
- [4. Create shell aliases](#4-create-shell-aliases)
- [5. Connect the V-Server with GitHub](#5-connect-the-v-server-with-github)
- [6. Configure multiple SSH identities](#6-configure-multiple-ssh-identities)

---
# 1. Create SSH access for the V-Server

* Create a new SSH key on your local machine
```bash
ssh-keygen -t ed25519
```

* Login to the server with username and password
```bash
ssh -i $HOME/.ssh/your-public-key.pub your-username@host
```

* Copy the public SSH key to the server

### Linux
```bash
ssh-copy-id -i $HOME/.ssh/your-public-key.pub your-username@host
```

### Windows
```cmd
type $HOME\.ssh\your-public-key.pub | ssh your-username@host "cat >> .ssh/authorized_keys"
```
* Test the SSH connection
```bash
ssh -i $HOME/.ssh/your-public-key.pub your-username@host
```

* Check the authorized SSH keys
```bash
cat ~/.ssh/authorized_keys
```

* Login again using the SSH key
```bash
ssh -i $HOME/.ssh/your-public-key.pub your-username@host
```

## Disable password login

* Open the SSH config file
```bash
sudo nano /etc/ssh/sshd_config
```

* Change this line:
```bash
#PasswordAuthentication yes
```
* To this:
```bash
#PasswordAuthentication no
```

* Save and close the file  
`CTRL + O` → `Enter` → `CTRL + X`

* Restart the SSH service
```bash
sudo systemctl restart ssh.service
```

* Logout from the server
```bash
logout
```

## Test the configuration

* Test login with the SSH key  
This should work:
```bash
ssh-copy-id -i $HOME/.ssh/your-public-key.pub your-username@host
```* Test login without public key authentication  
This should show `Permission denied`
```bash
ssh -o PubkeyAuthentication=no your-username@host
```

---

# 2. Install and start a web server

* Connect to the server
```bash
ssh-copy-id -i $HOME/.ssh/your-public-key.pub your-username@host
```

* Update the package list
```bash
sudo apt update
```

* Install Nginx
```bash
sudo apt install nginx -y
```* Check the Nginx status
```bash
systemctl status nginx.service
```

* Open the browser:
```bash
http://<your_ip_address>
```

You should now see the default Nginx page.

---

# 3. Change the Nginx default page

After installing Nginx, you can create your own custom page.

* Show the current Nginx page
```bash
sudo cat /var/www/html/index.nginx-debian.html
```* Add this configuration
```bash
server {
   listen 8081;
   listen [::]:8081;

   root /var/www/alternatives;
   index alternate-index.html;

   location / {
      try_files $uri $uri/ =404;
   }
}
```

## Edit the HTML page

* Open the HTML file
```bash
sudo nano /var/www/alternatives/alternate-index.html
```

* Add this example code
```html
<!DOCTYPE html>
<html>
<head>
      <meta charset="utf-8">
      <title>Hello nginx!</title>
</head>
<body>
      <h1>Hello nginx!</h1>
      <p>Nginx is now running on the Ubuntu server.</p>
</body>
</html>