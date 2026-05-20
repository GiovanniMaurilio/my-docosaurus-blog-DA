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
