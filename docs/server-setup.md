# Server Setup Documentation

## Generate SSH Keys

```bash
ssh-keygen -t ed25519
```

Generates a secure public/private SSH key pair stored inside your `.ssh` directory.

---

## Connect to the Server

```bash
ssh yourusername@your.server.ip
```

Logs into your remote server for the first time.  
Confirm the fingerprint with `yes` and enter your server password.

---

## Copy Your Public Key to the Server

```bash
ssh-copy-id -i <your/path/to/.ssh/V-Server/id_ed25519.pub> yourusername@your.server.ip
```

Adds your public SSH key to the server's `authorized_keys` file, enabling key‑based login.

---

## Test Key‑Based Login

```bash
ssh -i <your/path/to/.ssh/V-Server/id_ed25519> yourusername@your.server.ip
```

Ensures you can log in using your SSH key without a password.

---

## Disable Password Authentication

```bash
sudo nano /etc/ssh/sshd_config
```

Update the file and set:

```
PasswordAuthentication no
```

Apply changes:

```bash
sudo systemctl restart ssh.service
```

---

## Install NGINX

```bash
sudo apt update
sudo apt install nginx -y
```

Installs and updates the NGINX web server.

Check status:

```bash
systemctl status nginx.service
```

---

## Create an Alternative HTML Page

Create directory:

```bash
sudo mkdir /var/www/alternatives
```

Create HTML file:

```bash
sudo touch /var/www/alternatives/alternate-index.html
```

Add content:

```html
<!doctype html>
<html>
  <head>
    <meta charset="utf-8">
    <title>Hello, Nginx!</title>
  </head>
  <body>
    <h1>Hello, Nginx!</h1>
    <p>I have just configured our Nginx web server on Ubuntu Server!</p>
  </body>
</html>
```

Create new NGINX config:

```bash
sudo nano /etc/nginx/sites-enabled/alternatives
```

Add:

```nginx
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

Test configuration:

```bash
sudo nginx -t
```

Restart NGINX:

```bash
sudo service nginx restart
```

Access:

```
http://your.server.ip:8081
```

---

## Create SSH Aliases

```bash
alias v_server="ssh -i <your/path/to/.ssh/V-Server/id_ed25519> yourusername@your.server.ip"
```

Make the alias permanent:

```bash
nano ~/.zshrc
```

---

## Configure Multiple SSH Identities

Open SSH config:

```bash
vim <your/path/to/.ssh/config>
```

Add:

```text
Host vserver
    HostName your.server.ip
    User yourusername
    PreferredAuthentications publickey
    IdentityFile <your/path/to/.ssh/V-Server/id_ed25519>
```

Connect:

```bash
ssh vserver
```

---

## Configure GitHub SSH Access

Set your identity:

```bash
git config --global user.name "Your Name"
git config --global user.email "your.email@example.com"
```

Generate GitHub key:

```bash
ssh-keygen -t ed25519 -C "your.email@example.com" -f ~/.ssh/id_ed25519_github
```

Show public key:

```bash
cat ~/.ssh/id_ed25519_github.pub
```

Add on GitHub:  
**Settings → SSH and GPG keys → New SSH key**

Configure SSH:

```bash
nano ~/.ssh/config
```

Add:

```text
Host github.com
    HostName github.com
    User git
    IdentityFile ~/.ssh/id_ed25519_github
    IdentitiesOnly yes
```

Test:

```bash
ssh -T git@github.com
```

Expected output:

```
Hi yourgithubusername! You've successfully authenticated, but GitHub does not provide shell access.
```

---

✅ Your server is fully configured, secured, and connected to GitHub.