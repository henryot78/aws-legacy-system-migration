## Migrating a Company’s Legacy System to AWS

## Overview
This project demonstrates how to migrate a traditional **legacy web system** (previously hosted on a physical or outdated server) to **Amazon Web Services (AWS)** using a modern cloud-based architecture.

The migration replaces the legacy environment with an **AWS EC2 virtual server** running **Ubuntu**, hosting a full **LAMP stack** (Linux, Apache, MySQL, PHP) and deploying **WordPress** as the application layer.

The goal is to improve:
- Scalability 📈
- Security 🔐
- Reliability 🧱
- Maintainability 🛠️

---

## Architecture (High Level)
```
User Browser
     |
     v
Public Internet
     |
     v
AWS EC2 (Ubuntu)
 ├── Apache (Web Server)
 ├── PHP (Application Runtime)
 ├── WordPress (CMS)
 └── MySQL (Database)
```

---

## What This Project Simulates
In a real enterprise environment, this project represents:
- Migrating from an **on‑premise server** to the **cloud**
- Rebuilding infrastructure using **Infrastructure-as-a-Service (IaaS)**
- Separating concerns between:
  - Compute (EC2)
  - Application (WordPress / PHP)
  - Data (MySQL)

This is a foundational cloud migration pattern used by companies modernizing legacy systems.

---

## Tools & Technologies Used
| Category | Technology |
|------|-----------|
| Cloud Provider | AWS |
| Compute | EC2 |
| Operating System | Ubuntu |
| Web Server | Apache |
| Application Runtime | PHP |
| Database | MySQL |
| CMS | WordPress |
| Access Method | SSH |
| Package Manager | apt |

---

## Prerequisites
Before starting this project, you need:

### AWS Requirements
- AWS account
- EC2 key pair (`.pem` file)
- EC2 Ubuntu instance
- Security Group rules:
  - SSH (22) from your IP
  - HTTP (80) from `0.0.0.0/0`

### Local Machine Requirements
- macOS / Linux terminal (or Windows with SSH)
- Internet access
- Basic command-line familiarity

---

## Step-by-Step Implementation

### Step 1: Create EC2 Instance
- Launch an EC2 instance with **Ubuntu**
- Assign a key pair
- Configure security groups


**Outcome:** A running virtual server in AWS.

---

### Step 2: Connect to EC2 via SSH
```bash
chmod 400 your-key.pem
ssh -i "your-key.pem" ubuntu@ec2-xx-xx-xx-xx.compute.amazonaws.com
```

**Expected Result:**
- Successful login
- Terminal prompt shows:
  ```
  ubuntu@ip-172-31-xx-xx:~$
  ```

---

### Step 3: Update the Operating System
```bash
sudo apt update && sudo apt upgrade -y
```

**Why:** Ensures latest security patches and system updates.

---

### Step 4: Install Apache Web Server
```bash
sudo apt install apache2 -y
```

Verify:
```bash
sudo systemctl status apache2
```

**Expected Result:**
- Apache status: `active (running)`

---

### Step 5: Install PHP
```bash
sudo apt install php libapache2-mod-php php-mysql -y
```

Verify:
```bash
php -v
```

---

### Step 6: Install MySQL Database
```bash
sudo apt install mysql-server -y
```

---

### Step 7: Secure MySQL
```bash
sudo mysql_secure_installation
```

---

### Step 8: Download & Install WordPress
```bash
wget https://wordpress.org/latest.tar.gz
tar -xvzf latest.tar.gz
sudo mv wordpress/* /var/www/html/
```

---

### Step 9: Configure Permissions
```bash
sudo chown -R www-data:www-data /var/www/html/
sudo chmod -R 755 /var/www/html/
```

---

### Step 10: Create WordPress Database
```bash
sudo mysql
```

```sql
CREATE DATABASE wordpress_db;
CREATE USER 'wordpress_user'@'localhost' IDENTIFIED BY 'your_password';
GRANT ALL PRIVILEGES ON wordpress_db.* TO 'wordpress_user'@'localhost';
FLUSH PRIVILEGES;
EXIT;
```

---

### Step 11: Configure WordPress
```bash
sudo mv /var/www/html/wp-config-sample.php /var/www/html/wp-config.php
sudo nano /var/www/html/wp-config.php
```

---

### Step 12: Restart Apache & Finalize
```bash
sudo systemctl restart apache2
sudo rm /var/www/html/index.html
```

---

### Step 13: Access WordPress
```
http://<EC2_PUBLIC_IP>
```

---

## Validation Checklist
- Apache running ✔️
- MySQL running ✔️
- WordPress files present ✔️
- Website loads in browser ✔️

---

## Future Improvements
- Add HTTPS using Let’s Encrypt
- Use RDS instead of local MySQL
- Automate setup with Terraform
- Add monitoring with CloudWatch

---

## Author
Henry Omu
