```mysql
select @@global.sql_mode;
SET GLOBAL sql_mode='STRICT_TRANS_TABLES,NO_ZERO_IN_DATE,NO_ZERO_DATE,ERROR_FOR_DIVISION_BY_ZERO,NO_ENGINE_SUBSTITUTION';

FLUSH PRIVILEGES;

SHOW GRANTS FOR 'wfql'@'%';

CREATE USER 'wfql'@'%' IDENTIFIED BY 'wfql@2026!';

DROP USER 'wfql'@'%';

select host,user,PASSWORD from user;

GRANT SELECT ON machinecontroldb.* TO 'wfql'@'%';

UPDATE mysql.user SET host = 'localhost'WHERE user = 'root' AND host = 'localhost';
```



##### 定时清理数据库数据

```sql
-- 1. 开启事件调度器
SET GLOBAL event_scheduler = ON;

-- 2. 创建定时清理事件（每天凌晨2点执行）
CREATE EVENT clean_xxl_job_log
ON SCHEDULE EVERY 1 DAY STARTS CONCAT(CURDATE() + INTERVAL 1 DAY, ' 00:00:00')
DO
  DELETE FROM xxl_job_log 
  WHERE trigger_time < DATE_SUB(NOW(), INTERVAL 7 DAY);

-- 3. 查看已创建的事件
SHOW EVENTS;

-- 4. 如需删除事件
DROP EVENT IF EXISTS clean_xxl_job_log;
```

