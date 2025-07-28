# WSL Laravel Development Setup Guide

## Prerequisites
- WSL2 Ubuntu 22.04 installed
- User with sudo privileges (atau root user)

## 1. System Update & Basic Tools

```bash
sudo apt update && sudo apt upgrade -y
sudo apt install curl wget unzip git -y
```

## 2. PHP 8.4 Installation

```bash
# Add PHP repository
sudo add-apt-repository ppa:ondrej/php -y
sudo apt update

# Install PHP 8.4 with all required extensions
sudo apt install php8.4 php8.4-cli php8.4-common php8.4-mysql php8.4-zip php8.4-gd php8.4-mbstring php8.4-curl php8.4-xml php8.4-bcmath php8.4-tokenizer php8.4-ctype php8.4-fileinfo php8.4-intl php8.4-bz2 php8.4-calendar php8.4-exif php8.4-ftp php8.4-gettext php8.4-iconv php8.4-mysqli php8.4-pdo php8.4-phar php8.4-posix php8.4-readline php8.4-shmop php8.4-sockets php8.4-sysvmsg php8.4-sysvsem php8.4-sysvshm libapache2-mod-php8.4 -y

# Verify PHP version and extensions
php -v
php -m | grep -E "(xml|mysql|zip|gd|mbstring|curl)"
```

## 3. MySQL Installation & Configuration

```bash
# Install MySQL
sudo apt install mysql-server -y

# Run security installation
sudo mysql_secure_installation
# Pilih: No untuk VALIDATE PASSWORD
# Set password root: 'root' (atau kosong)
# Yes untuk remove anonymous users
# Yes untuk disallow root login remotely
# Yes untuk remove test database
# Yes untuk reload privilege tables

# Configure MySQL root for password authentication
sudo mysql -u root
```

```sql
ALTER USER 'root'@'localhost' IDENTIFIED WITH mysql_native_password BY 'root';
FLUSH PRIVILEGES;
EXIT;
```

```bash
# Test MySQL login
mysql -u root -p
# Password: root
```

## 4. Apache Installation & Configuration

```bash
# Install Apache
sudo apt install apache2 -y

# Enable PHP 8.4 module
sudo a2enmod php8.4
sudo systemctl restart apache2

# Test Apache
curl http://localhost
```

## 5. Composer Installation

```bash
# Download and install Composer globally
curl -sS https://getcomposer.org/installer -o /tmp/composer-setup.php
sudo php /tmp/composer-setup.php --install-dir=/usr/local/bin --filename=composer

# Verify Composer
composer --version
```

## 6. Node.js & npm Installation

```bash
# Install nvm (Node Version Manager)
curl -o- https://raw.githubusercontent.com/nvm-sh/nvm/v0.39.0/install.sh | bash
source ~/.bashrc

# Install Node.js LTS
nvm install --lts
nvm use --lts

# Verify installation
node --version
npm --version
```

## 7. phpMyAdmin Installation

```bash
# Install phpMyAdmin
sudo apt install phpmyadmin -y
# Select: apache2
# Select: Yes for configure database
# Set password atau kosong

# Install PHP 8.4 mbstring extension
sudo apt install php8.4-mbstring -y

# Enable phpMyAdmin configuration
sudo a2enconf phpmyadmin
sudo systemctl restart apache2

# Access: http://localhost/phpmyadmin
# Login: root / root
```

## 8. SSH Key Setup (for Git)

```bash
# Generate SSH key
ssh-keygen -t ed25519 -C "your-email@example.com"

# Copy public key
cat ~/.ssh/id_ed25519.pub

# Add to GitHub/GitLab SSH keys
# Test connection
ssh -T git@github.com
```

## 9. Project Directory Structure

```bash
# Create projects directory
mkdir ~/projects
cd ~/projects

# Clone repository
git clone git@github.com:username/repository.git
cd repository-name
```

## 10. Laravel Project Setup

```bash
# Install Composer dependencies
composer install

# Install NPM dependencies
npm install

# Environment configuration
cp .env.example .env
php artisan key:generate

# Configure database in .env
nano .env
```

### .env Database Configuration
```env
DB_CONNECTION=mysql
DB_HOST=127.0.0.1
DB_PORT=3306
DB_DATABASE=your_database_name
DB_USERNAME=root
DB_PASSWORD=root
```

```bash
# Create database
mysql -u root -p
```

```sql
CREATE DATABASE your_database_name;
EXIT;
```

```bash
# Run migrations
php artisan migrate

# Start development server
php artisan serve --port=8000
```

## 11. Multiple Branch Development

### Single Folder Approach:
```bash
cd ~/projects/repository-name

# Switch to dev branch
git checkout dev
git pull origin dev
```

### Multiple Folder Approach:
```bash
cd ~/projects

# Clone specific branch
git clone -b dev git@github.com:username/repository.git repository-dev
git clone -b main git@github.com:username/repository.git repository-main
```

## 12. Services Management

```bash
# Start all services
sudo systemctl start apache2
sudo systemctl start mysql

# Enable auto-start
sudo systemctl enable apache2
sudo systemctl enable mysql

# Check status
sudo systemctl status apache2
sudo systemctl status mysql
```

## 13. Access Points

- **Laravel App:** http://localhost:8000
- **phpMyAdmin:** http://localhost/phpmyadmin
- **Apache Default:** http://localhost

## 14. Tips & Tricks

### Tab Completion untuk Navigasi Cepat:
```bash
# Navigasi ke direktori dengan tab completion
cd ~/proj<TAB>  # Auto-complete ke ~/projects/
cd ~/projects/sim<TAB>  # Auto-complete ke nama repository

# Wildcard navigation
cd ~/projects/*laravel*  # Masuk ke folder yang mengandung "laravel"
cd ~/projects/sim*       # Masuk ke folder yang dimulai "sim"

# Recent directory navigation
cd -  # Kembali ke direktori sebelumnya
dirs -v  # Lihat history direktori
```

### Git Branch Management:
```bash
# Quick branch switching dengan tab completion
git checkout dev<TAB>
git checkout main<TAB>

# View all branches
git branch -a
```

### Laravel Artisan Shortcuts:
```bash
# Tab completion untuk artisan commands
php artisan mig<TAB>  # Auto-complete ke migrate
php artisan make:mod<TAB>  # Auto-complete ke make:model
```

## 15. Useful Commands

```bash
# PHP version check
php -v

# Check PHP modules
php -m

# MySQL status
sudo systemctl status mysql

# Apache status
sudo systemctl status apache2

# Laravel commands
php artisan migrate
php artisan migrate:refresh
php artisan serve --port=8000

# Git branch management
git branch -a
git checkout branch-name
git pull origin branch-name
```

## 16. Troubleshooting

### PHP Extension Issues:
```bash
# Install missing PHP extensions
sudo apt install php8.4-extension-name
sudo systemctl restart apache2

# Check which extensions are loaded
php -m

# Check PHP configuration files
php --ini

# Fix common Composer extension errors
sudo apt install php8.4-xml php8.4-dom php8.4-simplexml php8.4-xmlwriter php8.4-xmlreader

# If getting platform requirements error, install all common extensions:
sudo apt install php8.4-bz2 php8.4-calendar php8.4-exif php8.4-ftp php8.4-gettext php8.4-iconv php8.4-mysqli php8.4-pdo php8.4-phar php8.4-posix php8.4-readline php8.4-shmop php8.4-sockets php8.4-sysvmsg php8.4-sysvsem php8.4-sysvshm

# Temporary workaround for platform requirements
composer install --ignore-platform-req=ext-xml
```

### Permission Issues:
```bash
sudo chown -R $USER:$USER ~/projects
chmod -R 755 ~/projects
```

### Database Connection Issues:
```bash
# Reset MySQL root password
sudo mysql -u root
ALTER USER 'root'@'localhost' IDENTIFIED WITH mysql_native_password BY 'new_password';
FLUSH PRIVILEGES;
```

### Apache Issues:
```bash
# Check Apache error log
sudo tail -f /var/log/apache2/error.log

# Test Apache configuration
sudo apache2ctl configtest
```

## Notes

- WSL2 automatically forwards ports to Windows (no need for --host=0.0.0.0)
- Use `~/projects` directory for all development projects
- SSH keys are user-specific, generate for each user
- phpMyAdmin requires proper PHP extensions to function
- Use Tab completion for faster navigation: `cd ~/proj<TAB>`
- If getting PHP extension errors, install all required extensions with the commands in troubleshooting section