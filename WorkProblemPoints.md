#### com.github.pagehelper

```yml
<!-- pagehelper 分页插件 -->
<dependency>
    <groupId>com.github.pagehelper</groupId>
    <artifactId>pagehelper-spring-boot-starter</artifactId>
    <version>${pagehelper.boot.version}</version>
</dependency>
```

- 查询使用**limit**时，**pagehelper**会再次在sql语句后面添加limit，导致sql语法报错

- 报错之前写的代码

![image-20250228164947786](D:/Documents/guoqing.ling/Desktop/note/assets/image-20250228164947786.png)

![image-20250228164958650](D:/Documents/guoqing.ling/Desktop/note/assets/image-20250228164958650.png)

![image-20250228165008455](D:/Documents/guoqing.ling/Desktop/note/assets/image-20250228165008455.png)

- 修改后的代码，在查询之前写上 **PageHelper.clearPage();**，清除分页的缓存

![image-20250228165120620](D:/Documents/guoqing.ling/Desktop/note/assets/image-20250228165120620.png)