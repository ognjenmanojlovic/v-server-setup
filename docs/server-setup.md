# Server Setup Documentation

## 1. Generating SSH Keys and First Login
1. `ssh-keygen -t ed25519`  
   * Generates a public-private key pair.  
   * The files are saved locally (e.g. `/Users/yourname/.ssh/...`).  
   * Optionally secured with a passphrase.  

2. `ssh yourusername@your.server.ip`  
   * Connects to the server using the SSH protocol.  
   * Confirm the fingerprint with `yes` to trust the connection.  
   * Enter the server password to log in.

---

## 2. Adding the Public SSH Key to the Server
1. `ssh-copy-id -i /Users/yourname/.ssh/V-Server/id_ed25519.pub yourusername@your.server.ip`  
   * Copies the public key to the server’s `authorized_keys` file.  
   * This allows key-based authentication without a password.  

2. `ssh -i /Users/yourname/.ssh/V-Server/id_ed25519 yourusername@your.server.ip`  
   * Tests the connection using the private key.  
   * Successful authentication confirms the setup.  

---

## 3. Disabling Password Login
1. `sudo nano /etc/ssh/sshd_config`  
   * Edit the SSH configuration file.  
   * Change `#PasswordAuthentication yes` to `PasswordAuthentication no`.  

2. `sudo systemctl restart ssh.service`  
   * Restarts the SSH service to apply the change.  

3. `logout`  
   * Password login is now disabled.  
   * Only SSH key-based login is accepted.

---

## 4. Installing and Testing NGINX
1. `sudo apt update`  
   * Updates all available packages.  

2. `sudo apt install nginx -y`  
   * Installs the NGINX web server.  

3. `systemctl status nginx.service`  
   * Verifies if NGINX is running successfully.  
   * Visiting the server’s IP shows “Welcome to nginx!”.

---

## 5. Creating an Alternative HTML Page
1. `sudo mkdir /var/www/alternatives`  
   * Creates a new directory for the custom HTML page.  

2. `sudo touch /var/www/alternatives/alternate-index.html`  
   * Creates a new HTML file.  

3. Add content:
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

4. `sudo nano /etc/nginx/sites-enabled/alternatives`  
   * Create a new configuration file with:
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

5. `sudo nginx -t`  
   * Validates that the configuration syntax is correct.  

6. `sudo service nginx restart`  
   * Restarts NGINX to apply the new configuration.  
   * Access `http://your.server.ip:8081` to see your new HTML page.

---

## 6. Creating SSH Aliases
1. `alias v_server="ssh -i /Users/yourname/.ssh/V-Server/id_ed25519 yourusername@your.server.ip"`  
   * Defines a shortcut command for faster SSH access.  

2. Add the alias permanently by editing:  
   ```bash
   nano ~/.zshrc
   ```
   * Add the alias there to make it persistent.

---

## 7. Configuring Multiple SSH Identities
1. `vim /Users/yourname/.ssh/config`  
   * Edit SSH configuration for multiple hosts.  

2. Add:
   ```
   Host vserver
       HostName your.server.ip
       User yourusername
       PreferredAuthentications publickey
       IdentityFile /Users/yourname/.ssh/V-Server/id_ed25519
   ```

3. `ssh vserver`  
   * Connects directly using the saved configuration.

---

## 8. GitHub SSH Access on the V-Server
1. `git config --global user.name "Your Name"`  
   * Sets your Git author name for commits.  

2. `git config --global user.email "your.email@example.com"`  
   * Links commits to your GitHub account.  

3. `ssh-keygen -t ed25519 -C "your.email@example.com" -f ~/.ssh/id_ed25519_github`  
   * Generates a dedicated SSH key for GitHub access.  

4. `cat ~/.ssh/id_ed25519_github.pub`  
   * Displays the public key to copy into GitHub.  

5. Add the key on GitHub under  
   **Settings → SSH and GPG keys → New SSH key**  

6. `nano ~/.ssh/config`  
   * Add GitHub configuration:
   ```
   Host github.com
       HostName github.com
       User git
       IdentityFile ~/.ssh/id_ed25519_github
       IdentitiesOnly yes
   ```

7. `ssh -T git@github.com`  
   * Tests the connection.  
   * If successful, GitHub replies:  
     `Hi yourgithubusername! You've successfully authenticated, but GitHub does not provide shell access.`

---

✅ **The server is now fully configured, secured and connected to GitHub.**