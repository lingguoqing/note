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



##### 删除ACL

```sql
BEGIN
  DBMS_NETWORK_ACL_ADMIN.DROP_ACL(
    acl => 'qr_code_to_product_model.xml'
  );
  COMMIT;
EXCEPTION
  WHEN OTHERS THEN
    IF SQLCODE = -31001 THEN
      NULL; -- ACL不存在，忽略错误
    ELSE
      RAISE;
    END IF;
END;
```



##### 创建 http请求

```sql
--添加acl和权限控制（sql语句执行的方式来执行）
	begin
	  dbms_network_acl_admin.create_acl (       -- 创建访问控制文件（ACL）
		acl         => 'qr_code_to_product_model.xml',          -- 文件名称
		description => 'qr to product model',           -- 描述
		principal   => 'WFJT',             -- 授权或者取消授权账号，大小写敏感
		is_grant    => TRUE,                    -- 授权还是取消授权
		privilege   => 'connect',               -- 授权或者取消授权的权限列表
		start_date  => null,                    -- 起始日期
		end_date    => null                     -- 结束日期
	  );
	 
	  dbms_network_acl_admin.add_privilege (    -- 添加访问权限列表项
		acl        => 'qr_code_to_product_model.xml',           -- 刚才创建的acl名称 
		principal  => 'WFJT',                    -- 授权或取消授权用户
		is_grant   => TRUE,                     -- 与上同 
		privilege  => 'connect',                -- 权限列表
		start_date => null,                     
		end_date   => null
	  );
	 
	  dbms_network_acl_admin.assign_acl (       -- 该段命令意思是允许访问acl名为utl_http.xml下授权的用户，使用oracle网络访问包，所允许访问的目的主机，及其端口范围。
		acl        => 'qr_code_to_product_model.xml',
		host       => '10.1.111.24',           -- ip地址或者域名，填写https://localhost:9000/hello与https://localhost:9000/是会报host无效的
												-- 且建议使用ip地址或者使用域名，若用localhost，当oracle不是安装在本机上的情况下，会出现问题
		lower_port => 8888,                     -- 允许访问的起始端口号
		upper_port => Null                      -- 允许访问的截止端口号
	  );
	  commit;
	end;
```

##### 创建触发器

```sql
CREATE OR REPLACE TRIGGER QL_SF_QR_TO_PRODUCTMODE_CHANGE
AFTER INSERT ON QL_SF_QR_TO_PRODUCTMODE
FOR EACH ROW
DECLARE
    v_resp UTL_HTTP.RESP;
    v_req UTL_HTTP.REQ;
    v_QR_CODE VARCHAR2(30);
    v_url VARCHAR2(100);
BEGIN
    v_QR_CODE := :NEW.QR_CODE;
    v_url := 'http://10.1.111.24:8888/code/writeProductModel?qrCode=' || v_QR_CODE;
    
    v_req := UTL_HTTP.begin_request(v_url, 'GET', 'HTTP/1.1');
    DBMS_OUTPUT.put_line(v_url);
    v_resp := UTL_HTTP.get_response(v_req);
    UTL_HTTP.end_response(v_resp);
    
EXCEPTION
    WHEN OTHERS THEN
        DBMS_OUTPUT.put_line('Error: ' || SQLERRM);
END;
```

##### 插入时间触发器

```sql
CREATE OR REPLACE TRIGGER 触发器名称
BEFORE INSERT ON 表名
FOR EACH ROW
BEGIN
    :NEW.字段名 := SYSDATE;
END;
```

##### 插入并修改触发器

```sql
CREATE OR REPLACE TRIGGER 触发器名称
    BEFORE INSERT OR UPDATE ON 表名
    FOR EACH ROW
BEGIN
    -- 插入操作：设置创建时间和修改时间
    IF INSERTING THEN
        :NEW.字段名 := SYSDATE;
        :NEW.字段名 := SYSDATE;
    END IF;

    -- 更新操作：仅更新修改时间，创建时间保持不变
    IF UPDATING THEN
        :NEW.字段名 := SYSDATE;
    END IF;
END;
```



##### 删除重复值

```sql
DELETE FROM QL_SMART_FACTORY_TRACE
WHERE id IN (
 SELECT id FROM (
        SELECT t2.id
        FROM QL_SMART_FACTORY_TRACE t2
        INNER JOIN (
            SELECT TDCODE, MIN(id) AS min_id
            FROM QL_SMART_FACTORY_TRACE
            GROUP BY TDCODE
            HAVING COUNT(*) > 1
        ) dup ON t2.TDCODE = dup.TDCODE
        WHERE t2.id <> dup.min_id
    ) tmp
)
```

