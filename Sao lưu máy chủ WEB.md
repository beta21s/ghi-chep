
## Sao lưu trên máy tính local

```
#nano ~/.my.cnf

[mysql]
host=localhost
user=root
password=""

[mysqldump]
host=localhost
user=root
password=""

[mysqlhotcopy]
host=localhost
user=root
password=""

[mysqladmin]
host=localhost
user=root
password=""
```

### Sao lưu CSDL MySQL

```
#!/bin/bash

# Định nghĩa root
DBHOST=
DBUSER=
DBPASS=

#Thư mục lưu backup
BACKUPDIR=/mnt/mysql

# format is YYYYMMDD
DATE=`date +%Y-%m-%d` 
mkdir -p "$BACKUPDIR/$DATE"
cd "$BACKUPDIR/$DATE" 

MUTTXT=/mnt/mysql/$DATE/$DATE.txt

#Lấy list databases trên server
DBS=`mysql --host=$DBHOST -p$DBPASS -u $DBUSER --skip-column-names -e "show databases;" | awk  '{ print $1 }' | grep -v "information_schema"`

#Dumping database
for i in $DBS
do
        # format is dbname-YYYYMMDD.gz
        DBOUT=$i.sql.gz
        echo "$(date +%Y-%m-%d-%H-%M-%S) backing up $i to $DBOUT"
 
        #set umask to protect file
        umask 006      
 
        mysqldump -u $DBUSER -h $DBHOST -p$DBPASS --add-drop-table $i | gzip -9 - > $DBOUT
 
        echo "$(date +%Y-%m-%d-%H-%M-%S) backup successfully done. Please see attached file." >> $MUTTXT
        echo "$(date +%Y-%m-%d-%H-%M-%S) backup file: $DBOUT" >> $MUTTXT
done 

# Xóa các bản backup cũ hơn 14 ngày
echo "$(date +%Y-%m-%d-%H-%M-%S) deleting backups older than 4 days:"  >> $MUTTXT
find "$BACKUPDIR" -mindepth 1 -a -type d -mtime +4 -exec rm -rf {} \;
```

### Sao lưu mã nguồn

```
#!/bin/bash

# Thư mục cần backup
cit="/var/cit"

#Thư mục lưu backup
BACKUPDIR=/mnt/source

# format is YYYYMMDD
DATE=`date +%Y-%m-%d` 
mkdir -p "$BACKUPDIR/$DATE"
cd "$BACKUPDIR/$DATE" 

MUTTXT=/mnt/source/$DATE/$DATE.txt

# Backup CIT
echo "$(date +%Y-%m-%d-%H-%M-%S) start backup file in $cit." >> $MUTTXT
zip -r cit.zip $cit > /dev/null
echo "$(date +%Y-%m-%d-%H-%M-%S) backup file in $cit successfully." >> $MUTTXT

# Xóa các bản backup cũ hơn 6 ngày
echo "$(date +%Y-%m-%d-%H-%M-%S) deleting backups older than 3 days" >> $MUTTXT
find "$BACKUPDIR" -mindepth 1 -a -type d -mtime +3 -exec rm -rf {} \;
```

### Sao lưu file trên lên máy chủ FTP

```
#!/bin/bash

# FTP server settings
USERNAME=
PASSWORD=
SERVER=
PORT=

# Remote server directory to upload backup
BACKUPDIR="/faces-api"

# Backups older than ndays will be removed
ndays=7

# The absolute local directory to pickup *.tar.gz file
# Please do not end the path with a forward slash.
LOCAL_DIRECTORY="/home/ubuntu/faces-api/"

# The absolute directory path to store temporary files in.
# This must be granted write access for this script.
# Please do not end the path with a forward slash.
TEMP_BACKUP_STORE="/home/ubuntu"

# Please note that if you want to support encryption for backups, openssl must be installed.
# Should backups be encrypted before uploading?
ENCRYPT_BACKUP=false

# The absolute file path to the AES password.
# You can generate a random password using the following command:
# openssl rand -base64 256 > aes.key
# The number 256 is the amount of bytes to generate.
AES_PASSWORD_FILE=""

# END CONFIGURATION
# Script below, no need to modify it

timestamp=$(date --iso)

backup_remote_file_name="faces-api_$timestamp.tar.gz"
backup_file="$TEMP_BACKUP_STORE/$backup_remote_file_name"

# work out our cutoff date
MM=`date --date="$ndays days ago" +%b`
DD=`date --date="$ndays days ago" +%d`

echo "Removing files older than $MM $DD"

# get directory listing from remote source
listing=`ftp -i -n $SERVER $PORT <<EOMYF
user $USERNAME $PASSWORD
binary
cd $BACKUPDIR
ls
quit
EOMYF
`
lista=( $listing )

# loop over our files
for ((FNO=0; FNO<${#lista[@]}; FNO+=9));do
  # month (element 5), day (element 6) and filename (element 8)
  #echo Date ${lista[`expr $FNO+5`]} ${lista[`expr $FNO+6`]}          File: ${lista[`expr $FNO+8`]}

  # check the date stamp
  if [ ${lista[`expr $FNO+5`]}=$MM ];
  then
    if [[ ${lista[`expr $FNO+6`]} -lt $DD ]];
    then
      # Remove this file
      echo "Removing ${lista[`expr $FNO+8`]}"
      ftp -i -n $SERVER $PORT <<EOMYF2 
      user $USERNAME $PASSWORD
      binary
      cd $BACKUPDIR
      delete ${lista[`expr $FNO+8`]}
      quit
EOMYF2
    fi
  fi
done

echo "Creating backup..."

tar -czf $backup_file $LOCAL_DIRECTORY

if [ "$ENCRYPT_BACKUP" == "true" ]
then
      echo "Encrypting backup using OpenSSL..."
      output_encrypted_file="$backup_file.enc"
      openssl enc -aes-256-cbc -salt -in $backup_file -out $output_encrypted_file -pass file:$AES_PASSWORD_FILE
      rm $backup_file
      backup_file=$output_encrypted_file
fi

echo "Uploading backup $backup_file ..."

ftp -n -i $SERVER $PORT <<EOF
user $USERNAME $PASSWORD
cd $BACKUPDIR
put $backup_file $backup_remote_file_name
quit
EOF

echo "Deleting temporary files..."
rm $backup_file
echo "Backup complete."
```
