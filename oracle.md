# oracle 使用

- 查询所有的表

```sql
select * from tabs;
```

- 创建表空间

```sql
create tablespace waterboss(表名) datafile 'C:\waterboss.dbf'(存储路径) size 100m(初始大小) autoextend on next 10m(当存储大小达到100m后每次增加10m);
```

- 将表空间赋给某个用户

```sql
create user ling(用户名称) identified by 123456(密码) default tablespace waterboos(表名);
```

- 将该用户赋予权限

```sql
grant dba to ling(用户名称);
```



## 数据类型

#### 字符型

#### 数值型

#### 日期型

#### 二进制型（大数据类型）



## 导入\导出数据库（根据用户进行）

- 导出

```sq
exp system/123456(密码) owner=ling(用户名称) file=test.tmp(导出的文件名)
```

- 导入

```sql
imp syste/123456 file=test.tmp fromuser=ling
```



- 查看**SQL**索引命中情况

```sql
EXPLAIN PLAN for   SELECT * FROM ( SELECT TMP.*, ROWNUM ROW_ID FROM ( SELECT id,programname,programcontent,programpath,deviceprogrampath,createtime,updatetime,isdelete,devices_id,deliverystatus,reasonfailure FROM QL_SF_FANUC_PROGRAM_LOG WHERE isdelete=0 AND (devices_id = '2028661473435353089') ORDER BY updatetime DESC ) TMP WHERE ROWNUM <=10) WHERE ROW_ID > 0

select * from table(DBMS_XPLAN.display);
```

