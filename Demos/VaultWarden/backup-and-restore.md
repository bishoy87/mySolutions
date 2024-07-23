**Take a Backup from mariaDB and restore it in linux and windows**
Take backup:
```console
mysqldump -u root -p mydatabase > backup.sql
```

Restore backup:
```console
mysqldump -u root -p mydatabase < backup.sql --> didn't restore
mysql -u mysqlget -p vaultwarden < 1vault.sql   --> restroe successfully

```


Windows
Running: 
```console
mysql.exe --defaults-file="C:\Users\brizk\AppData\Local\Temp\tmp4m75y24d.cnf"  --protocol=tcp --host=192.168.56.21 --user=mysqlget --port=3306 --default-character-set=utf8 --comments --database=vaultwarden  < "D:\\Workspaces\\Devops\\Vagrant\\VaultWarden\\Sharedfolder\\1vault.sql"
```
