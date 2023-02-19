# MySQL

#### Cài đặt MySQL Server

```bash
sudo apt update
sudo apt install mysql-server
sudo systemctl start mysql.service
sudo mysql
```

#### Cài đặt phpMyAdmin

```bash
sudo apt-get install -y phpmyadmin php-mbstring php-gettext

# Chọn máy chủ Apache
```

<figure><img src="../.gitbook/assets/image.png" alt=""><figcaption></figcaption></figure>

#### Cấu hình MySQL

```
sudo mysql -u root -p
ALTER USER 'root'@'localhost' IDENTIFIED WITH mysql_native_password BY ‘123456’;

```

mysqli\_real\_connect(): (HY000/1698): Access denied for user 'root'@'localhost'
