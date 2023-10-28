# MongoDB

#### Cấp quyền

{% code overflow="wrap" %}
```mongodb
db.getSiblingDB("admin").updateUser(
    "root",
    {
        customData: {},
        roles: [{ "role": "root", "db": "admin" }, { "role": "clusterAdmin", "db": "admin" }, { "role": "readAnyDatabase", "db": "admin" }, { "role": "readWriteAnyDatabase", "db": "admin" }, { "role": "userAdminAnyDatabase", "db": "admin" }, { "role": "dbAdminAnyDatabase", "db": "admin" }, { "role": "userAdmin", "db": "admin" }, { "role": "dbAdmin", "db": "admin" }],
    }
)
```
{% endcode %}

#### Backup database

```mongodb
mongodump \
   --host=172.20.7.10 \
   --username=root \
   --password= \
   --out=
```

#### Tạo lịch sao lưu

{% code overflow="wrap" %}
```mongodb
0 2 * * * /usr/bin/mongodump --host=172.20.7.10 --username=root --password=rlaWTyS3fE6kI0A4 --gzip --archive=/home/ubuntu/mongo/backups/$(date +\%Y-\%m-\%d).dump
```
{% endcode %}
