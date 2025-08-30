# LAMP/LEMP Stack

## 🔹 Understanding LAMP & LEMP

- **LAMP = Linux + Apache + MySQL/MariaDB + PHP**
- **LEMP = Linux + Nginx (“Engine-X”) + MySQL/MariaDB + PHP**  

👉 Both stacks are used for hosting dynamic websites (like WordPress, Laravel, etc.).  
The only difference is **Apache (LAMP)** vs **Nginx (LEMP)** as the web server.

---

# 🟢 Step 1: Install Apache (for LAMP)

```bash
sudo apt update
sudo apt install apache2 -y
```

- Test Apache:  
  Open browser → `http://server-ip`  
  You should see **Apache2 Default Page**.

---

# 🟢 Step 2: Install Nginx (for LEMP)

```bash
sudo apt update
sudo apt install nginx -y
```

- Test Nginx:  
  Open → `http://server-ip`  
  You should see **Welcome to Nginx!** page.

---

# 🟢 Step 3: Install MySQL / MariaDB

```bash
sudo apt install mysql-server -y
sudo mysql_secure_installation
```

- Log into MySQL:
```bash
sudo mysql -u root -p
```

- Create DB & user:
```sql
CREATE DATABASE mydb;
CREATE USER 'myuser'@'localhost' IDENTIFIED BY 'mypassword';
GRANT ALL PRIVILEGES ON mydb.* TO 'myuser'@'localhost';
FLUSH PRIVILEGES;
EXIT;
```

---

# 🟢 Step 4: Install PHP

```bash
sudo apt install php libapache2-mod-php php-mysql -y   # for Apache
sudo apt install php-fpm php-mysql -y                  # for Nginx
```

- Check PHP version:
```bash
php -v
```

- Test PHP:
```bash
echo "<?php phpinfo(); ?>" | sudo tee /var/www/html/info.php
```
Now visit:  
👉 `http://server-ip/info.php`

---

# 🟢 Step 5: Configure Web Server

### For Apache (LAMP)
- Default web root: `/var/www/html`
- Restart Apache:
```bash
sudo systemctl restart apache2
```

### For Nginx (LEMP)
- Config file: `/etc/nginx/sites-available/default`
- Example config:
```nginx
server {
    listen 80;
    root /var/www/html;
    index index.php index.html;
    server_name your_domain.com;

    location / {
        try_files $uri $uri/ =404;
    }

    location ~ \.php$ {
        include snippets/fastcgi-php.conf;
        fastcgi_pass unix:/var/run/php/php8.1-fpm.sock;
    }

    location ~ /\.ht {
        deny all;
    }
}
```
- Restart Nginx:
```bash
sudo systemctl restart nginx
```

---

# 🟢 Step 6: Deploy Website (Example: WordPress)

```bash
cd /var/www/html
sudo rm index.nginx-debian.html
sudo wget https://wordpress.org/latest.tar.gz
sudo tar -xvzf latest.tar.gz
sudo mv wordpress/* .
sudo chown -R www-data:www-data /var/www/html
```

Then configure `wp-config.php` with DB details.

---

# 🔹 Summary

- **LAMP** → Apache (easier to configure, widely used).  
- **LEMP** → Nginx (faster, better for high traffic).  
- Both support MySQL/MariaDB + PHP.  
- Perfect for hosting WordPress, PHP apps, Laravel, etc.  
