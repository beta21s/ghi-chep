# Cấu hình hệ thống Ubuntu

#### Cấu hình thời gian

```
timedatectl 
timedatectl set-timezone Asia/Ho_Chi_Minh
Hoặc
sudo dpkg-reconfigure tzdata
```



```
apt install ntpdate
timedatectl set-timezone Asia/Ho_Chi_Minh
ntpdate -s ntp.ubuntu.com
```
