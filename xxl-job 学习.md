# xxl-job 使用（仅供参考）

- 官网地址：https://www.xuxueli.com/xxl-job/#%E3%80%8A%E5%88%86%E5%B8%83%E5%BC%8F%E4%BB%BB%E5%8A%A1%E8%B0%83%E5%BA%A6%E5%B9%B3%E5%8F%B0XXL-JOB%E3%80%8B

#### 下载**xxl-job**（版本可以根据自己的需求进行选择）

![image-20250312205022821](xxl-job%20%E5%AD%A6%E4%B9%A0.assets/image-20250312205022821.png)

![image-20250312205153214](xxl-job%20%E5%AD%A6%E4%B9%A0.assets/image-20250312205153214.png)

- 使用**idea**打开代码，在**doc文件夹**中找到**tables_xxl_job.sql**数据库，在本地将数据库创建号并将表导入

![image-20250312205353888](xxl-job%20%E5%AD%A6%E4%B9%A0.assets/image-20250312205353888.png)

- 修改数据库的用户名和密码
- 启动xxl-job服务即可，启动后的页面

![image-20250312205516761](xxl-job%20%E5%AD%A6%E4%B9%A0.assets/image-20250312205516761.png)

#### 在自己的项目中如何使用（也可以查看官方文档进行配置）

- 首先导入**xxl-job**依赖，与下载的xxl-job版本一致即可

```xml
<!-- xxl-job-core -->
<dependency>
    <groupId>com.xuxueli</groupId>
    <artifactId>xxl-job-core</artifactId>
    <version>3.0.0</version>
</dependency>
```

- 在yml中进行xxl-job的配置

```yml
xxl:
  job:
    admin:
      #  对应的是xxl-job项目启动的端口与地址
      addresses: http://127.0.0.1:9900/xxl-job-admin
      ### xxl-job, access token
      accessToken: default_token
      ### xxl-job timeout by second, default 3s
    #      imeout: 3
    ### xxl-job executor appname
    executor:
      appname: ${spring.application.name}
      ### xxl-job executor registry-address: default use address to registry , otherwise use ip:port if address is null
      address:
      ### xxl-job executor server-info
      ip:
      port: 9901
      ### xxl-job executor log-path
      logpath: /data/applogs/xxl-job/jobhandler
      ### xxl-job executor log-retention-days
      logretentiondays: 30
```

- 实现一个配置类（也可以直接复制官方文档的）

```java
@Configuration
public class XxlJobConfig {
    private Logger logger = LoggerFactory.getLogger(XxlJobConfig.class);

    @Value("${xxl.job.admin.addresses}")
    private String adminAddresses;

    @Value("${xxl.job.admin.accessToken}")
    private String accessToken;

/*    @Value("${xxl.job.admin.timeout}")
    private int timeout;*/

    @Value("${xxl.job.executor.appname}")
    private String appname;

    @Value("${xxl.job.executor.address}")
    private String address;

    @Value("${xxl.job.executor.ip}")
    private String ip;

    @Value("${xxl.job.executor.port}")
    private int port;

    @Value("${xxl.job.executor.logpath}")
    private String logPath;

    @Value("${xxl.job.executor.logretentiondays}")
    private int logRetentionDays;


    @Bean
    public XxlJobSpringExecutor xxlJobExecutor() {
        logger.info(">>>>>>>>>>> xxl-job config init.");
        XxlJobSpringExecutor xxlJobSpringExecutor = new XxlJobSpringExecutor();
        xxlJobSpringExecutor.setAdminAddresses(adminAddresses);
        xxlJobSpringExecutor.setAppname(appname);
        xxlJobSpringExecutor.setAddress(address);
        xxlJobSpringExecutor.setIp(ip);
        xxlJobSpringExecutor.setPort(port);
        xxlJobSpringExecutor.setAccessToken(accessToken);
//        xxlJobSpringExecutor.setTimeout(timeout);
        xxlJobSpringExecutor.setLogPath(logPath);
        xxlJobSpringExecutor.setLogRetentionDays(logRetentionDays);

        return xxlJobSpringExecutor;
    }

    /**
     * 针对多网卡、容器内部署等情况，可借助 "spring-cloud-commons" 提供的 "InetUtils" 组件灵活定制注册IP；
     *
     *      1、引入依赖：
     *          <dependency>
     *             <groupId>org.springframework.cloud</groupId>
     *             <artifactId>spring-cloud-commons</artifactId>
     *             <version>${version}</version>
     *         </dependency>
     *
     *      2、配置文件，或者容器启动变量
     *          spring.cloud.inetutils.preferred-networks: 'xxx.xxx.xxx.'
     *
     *      3、获取IP
     *          String ip_ = inetUtils.findFirstNonLoopbackHostInfo().getIpAddress();
     */

}
```

- 启动自己的项目即可

#### 页面配置

- 调度中心新增栏目 “执行器管理” : 管理在线的执行器, 通过属性AppName自动发现注册的执行器。只有被管理的执行器才允许被使用
- 在执行器页面中进行新增执行器，将刚刚自己项目中配置的**xxl.job.executor.appname**的值填上

![image-20250312210657440](xxl-job%20%E5%AD%A6%E4%B9%A0.assets/image-20250312210657440.png)

- 等待自己的项目注册上去，如果没有可以多次刷新或者重新登陆

**![image-20250312210801633](xxl-job%20%E5%AD%A6%E4%B9%A0.assets/image-20250312210801633.png)**



#### 开发Job任务

- 任务开发

  - 任务开发：在Spring Bean实例中，开发Job方法；

  ```java
  @Slf4j
  @Component
  public class SampleXxlJob {
  
  
      @XxlJob("dynamicLogJobHandler")
      public ReturnT<String> dynamicLogJobHandler() {
          // 记录实时日志（直接显示在调度日志详情中）
          XxlJobHelper.log("--- 任务开始执行 ---");
  
          try {
              // 阶段1：数据预处理
              XxlJobHelper.log("开始预处理数据...");
  
  
              // 阶段2：业务计算
              XxlJobHelper.log("开始业务计算...");
              // 手动标记成功，并附加自定义描述（覆盖默认的msg）
              XxlJobHelper.handleSuccess("任务完成，处理数据量：1000条");
              return ReturnT.SUCCESS;
          } catch (Exception e) {
              // 手动标记失败，并记录异常信息
              XxlJobHelper.log("捕获异常：" + e.getMessage());
              XxlJobHelper.handleFail("任务失败，原因：" + e.getMessage());
              return ReturnT.FAIL;
          }
      }
  }
  ```

- 执行日志：需要通过 "XxlJobHelper.log" 打印执行日志；

- 任务结果：默认任务结果为 "成功" 状态，不需要主动设置；如有诉求，比如设置任务结果为失败，可以通过 "XxlJobHelper.handleFail/handleSuccess" 自主设置任务结果；

- 在任务管理页面中进行新增定时任务

![image-20250312211246499](xxl-job%20%E5%AD%A6%E4%B9%A0.assets/image-20250312211246499.png)

- 在cron设置任务时间，**JobHandler***对应的就是代码中@XxlJob里面的值

![image-20250312211402762](xxl-job%20%E5%AD%A6%E4%B9%A0.assets/image-20250312211402762.png)

- 点击执行一次后，任务便会执行，可以在调度日志中进行查看任务执行的状态

![image-20250312211518415](xxl-job%20%E5%AD%A6%E4%B9%A0.assets/image-20250312211518415.png)

- 也可以通过执行日志进行详细查看

![image-20250312211552350](xxl-job%20%E5%AD%A6%E4%B9%A0.assets/image-20250312211552350.png)

![image-20250312211601438](xxl-job%20%E5%AD%A6%E4%B9%A0.assets/image-20250312211601438.png)