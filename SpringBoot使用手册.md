## Maven
spring-boot-starter-parent提供以下默认属性：
- 默认使用Java 1.8
- UTF-8编码

*application.properties和application.yml可以使用Spring风格的占位符（${...}），maven使用@..@作为占位符。*
使用SpringBoot的maven插件可以将项目打包打包为可执行的jar。
><build>
	<plugins>
		<plugin>
			<groupId>org.springframework.boot</groupId>
			<artifactId>spring-boot-maven-plugin</artifactId>
		</plugin>
	</plugins>
</build>

## Starters
Starter的命名规范：
- SpringBoot官方的Starter都是以spring-boot-starter-* 方式命名，第三方自定义的starter都是 *-spring-boot-starter方式命名。

## Importing Additional Configuration Classes
如果所有的配置不是放在一个配置类当中，可以使用@Import注解引入其他的配置类，或者配置@ComponentScan扫描有@Configuration的配置类。

## 自动配置
通过添加@EnableAutoConfiguration或@SpringBootApplication注解，SpringBoot将根据你添加依赖jar自动配置。

## 替换自动配置
可以通过自定配置bean的方式来替换自动配置，如果想查看当前应用的自动配置信息，可以在启动的时候添加--debug参数在控制台输出配置信息。

## 关闭指定自动配置类
可以通过@EnableAutoConfiguration注解exclude属性关闭指定自动配置类。
``` java
import org.springframework.boot.autoconfigure.*;
import org.springframework.boot.autoconfigure.jdbc.*;
import org.springframework.context.annotation.*;

@Configuration
@EnableAutoConfiguration(exclude={DataSourceAutoConfiguration.class})
public class MyConfiguration {
}
```
你也通过excludeName指定配置类的全限定名的方式来关闭自动配置，你还可以通过配置文件spring.autoconfigure.exclude属性来指定需要关闭的自动配置.

## Spring Beans和依赖注入
如果将SpringBoot主配置类放在包的根目录下，所有的有@Component, @Service, @Repository, @Controller等注解的类会被自动扫描并注入到Spring Beans中。

## @SpringBootApplication注解
@SpringBootApplication注解相当于@Configuration, @EnableAutoConfiguration和@ComponentScan三个注解一起使用。

## 运行SpringBoot项目
你可以通过以下命令运行打包成jar的SpringBoot项目：
```
$ java -jar target/myapplication-0.0.1-SNAPSHOT.jar
```
你也可以通过以下命令开启远程调试：
```
$ java -Xdebug -Xrunjdwp:server=y,transport=dt_socket,address=8000,suspend=n \
       -jar target/myapplication-0.0.1-SNAPSHOT.jar
```

## Developer Tools
SpringBoot提供了一些开发工具在开发环境中使用，引入SpringBoot开发工具：
```
<dependencies>
	<dependency>
		<groupId>org.springframework.boot</groupId>
		<artifactId>spring-boot-devtools</artifactId>
		<optional>true</optional>
	</dependency>
</dependencies>
```
### spring-boot-devtools的默认属性
SpringBoot有些库会使用缓存来提供性能，比如模板引擎会缓存编译结果来避免重复的转换。但是在开发环境中这种缓存是没有必要的，所以spring-boot-devtools默认关闭了这些缓存。你可以在你的application.properties配置文件中，定义这些缓存行为，比如：Thymeleaf缓存通过spring.thymeleaf.cache属性控制。 
在开发环境中你可能需要详细请求信息，devtools默认将web logging group的日志级别设置为DEBUG，如果你需要更详细的请求和响应信息，你可以设置spring.http.log-request-details配置属性为true。

### 自动重启
spring-boot-devtools将会自动重启当classpath目录下的文件有变化时。默认情况下，会监控classpath目录下的所有文件。
>**怎么样触发重启？** 只有classpath下文件更新才会重发重启。eclipse里面保存一个被修改的文件将会是classpath下的文件更新，在IntelliJ IDEA中编译项目(Build -> Build Project) 也会更新classpath下的文件。
