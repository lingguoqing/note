# maven 命令使用

- 将jar 导入到本地maven仓库

```cmd
mvn install:install-file -Dfile=artemis-http-client-1.1.13.RELEASE.jar -DgroupId=com.hikvision.ga -DartifactId=artemis-http-client -Dversion=1.1.13.RELEASE -Dpackaging=jar
```

