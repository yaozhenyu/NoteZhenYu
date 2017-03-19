## 修改端口

- 1、Java代码修改
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

*classPath:application.properties*文件中
```
server.port=9100
```
