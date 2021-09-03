```
-- master db
use master
-- create SQL login in master database
CREATE LOGIN testLogin1
WITH PASSWORD = 'Azure@123456';
```

```
-- user db
use sqlpool
-- add database user for login testLogin1
CREATE USER [testLogin1]
FROM LOGIN [testLogin1]
WITH DEFAULT_SCHEMA=dbo;
```


```
-- add user to role(s) in db 
EXEC sp_addrolemember 'db_datareader', 'testLogin1';
EXEC sp_addrolemember 'db_datawriter', 'testLogin1';
```
