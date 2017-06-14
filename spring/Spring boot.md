- 1、Java代码修改端口
```
package hello;

import org.springframework.boot.SpringApplication;
import org.springframework.boot.autoconfigure.SpringBootApplication;

@SpringBootApplication
public class Application {
	public static void main(String[] args){
		System.setProperty("server.port", "9100");
		SpringApplication.run(Application.class, args);
	}
}
```

- 2、配置文件

*classPath:application.properties*文件,或application.yml
```
# 指定启动端口号
server.port=9100
# 指定起作用的配置文件为application-dev.properties或application-dev.yml
spring.profiles.active=dev 

# 数据库配置
spring.datasource.url=jdbc:mysql://localhost:3306/test?characterEncoding=utf8
spring.datasource.username=root
spring.datasource.password=root

# Jpa配置
spring.jpa.database=MYSQL
spring.jpa.show-sql=true
# 每次启动检查表结构
spring.jpa.hibernate.ddl-auto=update
spring.jpa.hibernate.naming.strategy=org.hibernate.cfg.ImprovedNamingStrategy
# 设置方言
spring.jpa.properties.hibernate.dialect=org.hibernate.dialect.MySQL5Dialect
```



* 3、单元测试,加注解，无需做其他配置

  ```
  @RunWith(SpringJUnit4ClassRunner.class)
  @SpringBootTest
  ```

  ​