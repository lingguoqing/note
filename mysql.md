```mysql
select @@global.sql_mode;
SET GLOBAL sql_mode='STRICT_TRANS_TABLES,NO_ZERO_IN_DATE,NO_ZERO_DATE,ERROR_FOR_DIVISION_BY_ZERO,NO_ENGINE_SUBSTITUTION';

FLUSH PRIVILEGES;

SHOW GRANTS FOR 'wfql'@'%';

CREATE USER 'wfql'@'%' IDENTIFIED BY 'wfql@2026!';

DROP USER 'wfql'@'%';

select host,user,PASSWORD from user;

GRANT SELECT ON machinecontroldb.* TO 'wfql'@'%';

```

