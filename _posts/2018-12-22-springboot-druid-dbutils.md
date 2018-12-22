---
title: "spring boot 整合druid"
tags: [spring boot, druid, dbutils]
excerpt: "最近一直工作中突然要用到P端交易实时监控，所以正好记录下碰到的问题"
---

------------

需要实现的功能其实是比较简单的，就是在C端交易，通过P端转发，当该笔交易完成之后，P端进行数据库记录，并实时显示在银行监控端。

其实就是需要不断去数据库查询当前的是时候交易记录即可。



## 1.druid
参考文档：

[spring boot 开发druid 配置]<br/>
[1]: https://yq.aliyun.com/articles/552746?spm=5176.10695662.1996646101.searchclickresult.757a1f46fqBXoN  
[2]: https://blog.csdn.net/scholar_man/article/details/78844172


关于druid 有个问题一直都没想好怎么解决：正常连接启动之后，由于网络问题导致连接突然断开了，会直接报错，怎么可以把这种异常信息抓到，并显示通知页面客户
```
pom.xml 配置

<dependency>
    <groupId>com.alibaba</groupId>
    <artifactId>druid</artifactId>
    <version>${druid-version}</version>
</dependency>

在进入druid之后，sql监控不能正常显示数据，在其他文章中也有看到使用下面这个，（这个主要还是跟配置的拦截器有关系），
因为我这边基本上直接在java类中sql是固定的，所以这边也拦截不到其他的请求，所以数据一直都没有正常显示。个人理解，欢迎指正。

<dependency>
    <groupId>com.alibaba</groupId>
    <artifactId>druid-spring-boot-starter</artifactId>
    <version>1.1.1</version>
</dependency>

```

application.properties
```
#DB2配置
spring.datasource.type=com.alibaba.druid.pool.DruidDataSource
spring.datasource.driver-class-name=com.ibm.db2.jcc.DB2Driver
spring.datasource.url=jdbc:db2://10.1.2.74:60052/db2atm1
spring.datasource.username=db2atm1
spring.datasource.password=db2atm1

# 下面为连接池的补充设置，应用到上面所有数据源中
# 初始化大小，最小，最大
spring.datasource.initialSize=5
spring.datasource.minIdle=5
spring.datasource.maxActive=20
# 配置获取连接等待超时的时间
spring.datasource.maxWait=60000
# 配置间隔多久才进行一次检测，检测需要关闭的空闲连接，单位是毫秒
spring.datasource.timeBetweenEvictionRunsMillis=60000
# 配置一个连接在池中最小生存的时间，单位是毫秒
spring.datasource.minEvictableIdleTimeMillis=300000
spring.datasource.validationQuery=SELECT 1 FROM DUAL
spring.datasource.testWhileIdle=true
spring.datasource.testOnBorrow=false
spring.datasource.testOnReturn=false
# 打开PSCache，并且指定每个连接上PSCache的大小
spring.datasource.poolPreparedStatements=true
spring.datasource.maxPoolPreparedStatementPerConnectionSize=20
# 配置监控统计拦截的filters，去掉后监控界面sql无法统计，'wall'用于防火墙
spring.datasource.filters=stat,wall,log4j
# 通过connectProperties属性来打开mergeSql功能；慢SQL记录
spring.datasource.connectionProperties=druid.stat.mergeSql=true;
druid.stat.slowSqlMillis=5000
spring.datasource.logSlowSql=true
# 合并多个DruidDataSource的监控数据
spring.datasource.useGlobalDataSourceStat=true
spring.main.allow-bean-definition-overriding=true

```

工具类：DruidUtils
```
package com.qhnx.Config;

import com.alibaba.druid.pool.DruidDataSource;
import com.alibaba.druid.support.http.StatViewServlet;
import com.alibaba.druid.support.http.WebStatFilter;
import org.slf4j.Logger;
import org.slf4j.LoggerFactory;
import org.springframework.beans.factory.annotation.Value;
import org.springframework.boot.web.servlet.FilterRegistrationBean;
import org.springframework.boot.web.servlet.ServletRegistrationBean;
import org.springframework.context.annotation.Bean;
import org.springframework.context.annotation.Configuration;

import javax.sql.DataSource;
import java.sql.SQLException;


@Configuration
public class DruidConfig {

    private Logger logger = LoggerFactory.getLogger(DruidConfig.class);

    @Value("${spring.datasource.url}")
    private String dbUrl;

    @Value("${spring.datasource.username}")
    private String username;

    @Value("${spring.datasource.password}")
    private String password;

    @Value("${spring.datasource.driver-class-name}")
    private String driverClassName;

    @Value("${spring.datasource.initialSize}")
    private int initialSize;

    @Value("${spring.datasource.minIdle}")
    private int minIdle;

    @Value("${spring.datasource.maxActive}")
    private int maxActive;

    @Value("${spring.datasource.maxWait}")
    private int maxWait;

    @Value("${spring.datasource.timeBetweenEvictionRunsMillis}")
    private int timeBetweenEvictionRunsMillis;

    @Value("${spring.datasource.minEvictableIdleTimeMillis}")
    private int minEvictableIdleTimeMillis;

    @Value("${spring.datasource.validationQuery}")
    private String validationQuery;

    @Value("${spring.datasource.testWhileIdle}")
    private boolean testWhileIdle;

    @Value("${spring.datasource.testOnBorrow}")
    private boolean testOnBorrow;

    @Value("${spring.datasource.testOnReturn}")
    private boolean testOnReturn;

    @Value("${spring.datasource.filters}")
    private String filters;

    @Value("${spring.datasource.logSlowSql}")
    private String logSlowSql;

    @Bean
    public ServletRegistrationBean druidServlet() {
        ServletRegistrationBean reg = new ServletRegistrationBean();
        reg.setServlet(new StatViewServlet());
        reg.addUrlMappings("/druid/*");
        reg.addInitParameter("loginUsername", username);
        reg.addInitParameter("loginPassword", password);
        reg.addInitParameter("logSlowSql", logSlowSql);
        return reg;
    }

    @Bean
    public FilterRegistrationBean filterRegistrationBean() {
        FilterRegistrationBean filterRegistrationBean = new FilterRegistrationBean();
        filterRegistrationBean.setFilter(new WebStatFilter());
        filterRegistrationBean.addUrlPatterns("/*");
        filterRegistrationBean.addInitParameter("exclusions", "*.js,*.gif,*.jpg,*.png,*.css,*.ico,/druid/*");
        filterRegistrationBean.addInitParameter("profileEnable", "true");
        return filterRegistrationBean;
    }

    @Bean
    public DataSource druidDataSource() {
        DruidDataSource datasource = new DruidDataSource();
        datasource.setUrl(dbUrl);
        datasource.setUsername(username);
        datasource.setPassword(password);
        datasource.setDriverClassName(driverClassName);
        datasource.setInitialSize(initialSize);
        datasource.setMinIdle(minIdle);
        datasource.setMaxActive(maxActive);
        datasource.setMaxWait(maxWait);
        datasource.setTimeBetweenEvictionRunsMillis(timeBetweenEvictionRunsMillis);
        datasource.setMinEvictableIdleTimeMillis(minEvictableIdleTimeMillis);
        datasource.setValidationQuery(validationQuery);
        datasource.setTestWhileIdle(testWhileIdle);
        datasource.setTestOnBorrow(testOnBorrow);
        datasource.setTestOnReturn(testOnReturn);
        try {
            datasource.setFilters(filters);
        } catch (SQLException e) {
            logger.error("druid configuration initialization filter", e);
        }
        return datasource;
    }

}
```

如果整合成功，则可以直接进入 http://localhost:8080/druid/weburi.html  直接进行登录。用户名和密码则是配置文件中的数据库名和密码
如果不想使用，也可以在工具类中进行修改即可。

# 2.Apach dbutils

参考文档：  
[apach dbutils 配置]<br/>
[1]: https://blog.csdn.net/earbao/article/details/44901061  
[2]: https://blog.csdn.net/softwave/article/details/18604565  
[3]:http://commons.apache.org/


在网上查了好多种方式,感觉这个用起来更方便一点。直接贴代码：  
```
<dependency>
   <groupId>commons-dbutils</groupId>
   <artifactId>commons-dbutils</artifactId>
   <version>1.6</version>
</dependency>
```
```
package com.qhnx.Config;

import java.io.InputStream;
import java.util.Properties;
import javax.sql.DataSource;

import com.alibaba.druid.pool.DruidDataSource;
import com.alibaba.druid.pool.DruidDataSourceFactory;

public class DruidUtils {
    private static DruidDataSource ds;

    static {
        try {
            InputStream is = DruidUtils.class.getClassLoader().getResourceAsStream("application.properties");
            Properties properties = new Properties();
            properties.load(is);

            ds = (DruidDataSource) DruidDataSourceFactory.createDataSource(properties);
        } catch (Exception e) {
            e.printStackTrace();
        }
    }

    public static DataSource getDataSource() {
        if (ds != null) {
            return ds;
        }
        return null;
    }
}


```
**在这个地方有个特别需要注意：读取数据源配置文件的时候，这边默认读取的名称为'driverClassName,url,username,password',所以就算之前在整合druid已经配置了数据源，但是在这里建议重新进行如下配置**
```
driverClassName=com.ibm.db2.jcc.DB2Driver
url=jdbc:db2://10.1.2.74:60052/db2atm1
username=db2atm1
password=db2atm1
```


```
package com.qhnx.dao.impl;

import com.qhnx.Config.DruidUtils;
import com.qhnx.dao.ITranRecordDao;
import com.qhnx.model.monitor;

import org.apache.commons.dbutils.*;
import org.apache.commons.dbutils.handlers.BeanListHandler;


import java.sql.SQLException;

import java.util.List;

public class TranRecordDaoImpl implements ITranRecordDao {


    /**
     * 查找所有
     *
     * @return
     */

    @Override

    public List<monitor> findAll() {

        QueryRunner runner = new QueryRunner(DruidUtils.getDataSource());
        List<monitor>  list = null;

        try {
            String SQL = "select * from TRANREC_MONITOR fetch first 50 rows only";
            list = runner.query(SQL, new BeanListHandler<>(monitor.class));
        } catch (SQLException e) {
            e.printStackTrace();


        }
        return list;

    }

}

说明：BeanListHandler 查询出多个结果集。

```

# 3. spring boot

在这里只说明一个启动时报错的问题。由于当时没准备写这个说明，所以没有错误截图。

在启动的时候如果报错找不到配置文件，或者找不到数据源的配置文件，类似于这种情况，只要在启动项增加  

@SpringBootApplication(exclude={DataSourceAutoConfiguration.class,HibernateJpaAutoConfiguration.class})


其他的错误问题都比较常见可以直接搜到。




最后说个数据源驱动的问题：   
因为我这边服务器使用的db2数据库，自己电脑安装的mysql,无意中发现的问题。  
mysql在使用驱动时需要注意，否则启动时会报错
```
在6.0.2版本之前
driver=com.mysql.jdbc.Driver    
url=jdbc:mysql://host:port/dbname?characterEncoding=utf8

在6.0.2版本之后
driver=com.mysql.cj.jdbc.Driver
url=jdbc:mysql://host:port/dbname?characterEncoding=utf8&serverTimezone=UTC

<dependency>
    <groupId>com.ibm.db2</groupId>
    <artifactId>db2jcc4</artifactId>
    <version>10.5</version>
</dependency>
<dependency>
    <groupId>com.ibm.db2</groupId>
    <artifactId>db2jcc_license_cu</artifactId>
    <version>10.5</version>
</dependency>

<dependency>
    <groupId>mysql</groupId>
    <artifactId>mysql-connector-java</artifactId>
</dependency>
```
