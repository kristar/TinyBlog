title: log4j2-LearnNote
date: 2017/06/22 01:43
comments: true
categories:
  - Java
tags:
  - log4j2
---
# 简单例子

**手动导包**

> log4j-api-2.x.jar
>
> log4j.core-2.x.jar



**Maven导包**

```xml
<dependencies>  
    <dependency>
	  <groupId>org.apache.logging.log4j</groupId>
	  <artifactId>log4j-api</artifactId>
	  <version>2.8.2</version>
	</dependency>
	<dependency>
	  <groupId>org.apache.logging.log4j</groupId>
	  <artifactId>log4j-core</artifactId>
	  <version>2.8.2</version>
	</dependency>
</dependencies>
```



**测试代码**

```java
public static void main(String[] args) {  
    Logger logger = LogManager.getLogger(LogManager.ROOT_LOGGER_NAME);  
    logger.trace("trace level");  
    logger.debug("debug level");  
    logger.info("info level");  
    logger.warn("warn level");  
    logger.error("error level");  
    logger.fatal("fatal level");  
} 
```



**输出**

> 1. ERROR StatusLogger No log4j2 configuration file found. Using default configuration: logging only errors to the console.  
> 2. 20:37:11.965 [main] ERROR  - error level  
> 3. 20:37:11.965 [main] FATAL  - fatal level  

第一句是没有找到 log4j2 的配置文件, 然后使用了默认配置文件, 只会将error级别的日志打印到控制台上

添加一个配置文件 log4j2.xml 到 classpath 中(src 根目录)

```xml
<?xml version="1.0" encoding="UTF-8"?>
<!-- status: log4j2的日志信息打印级别 -->
<Configuration status="WARN">  
    <Appenders>  
        <Console name="Console" target="SYSTEM_OUT">  
            <PatternLayout pattern="%d{HH:mm:ss.SSS} [%t] %-5level %logger{36} - %msg%n" />  
        </Console>  
    </Appenders>  
    <Loggers>  
        <Root level="error">  
            <AppenderRef ref="Console" />  
        </Root>  
    </Loggers>  
</Configuration> 
```

日志级别 level 由低到高: TRACE < DEBUG < INFO < WARN < ERROR < FATAL, 如果设置低级别都可以打印, 那么高级别肯定会打印.

Appender是日志追加的位置, Console 指的是输出到控制台, PatternLayout 配置的是输出的格式

- %d{HH:mm:ss.SSS} 表示输出到毫秒的时间
- %t 输出当前线程名称
- %-5level 输出日志级别，-5表示左对齐并且固定输出5个字符，如果不足在右边补0
- %logger 输出logger名称，因为Root Logger没有名称，所以没有输出
- %msg 日志文本
- %n 换行

其他常用的占位符有：

- %F 输出所在的类文件名，如Client.java
- %L 输出行号
- %M 输出所在方法名
- %l  输出语句所在的行数, 包括类名、方法名、文件名、行数

还要配置 Logger, 必须配置一个 Root Logger, 上面的 Java 代码就是通过通过 Root Logger 的名字获取 Logger



# 自定义 Logger

**Java代码**

```java
public static void main(String[] args) {  
    Logger logger = LogManager.getLogger("mylog");  
    logger.trace("trace level");  
    logger.debug("debug level");  
    logger.info("info level");  
    logger.warn("warn level");  
    logger.error("error level");  
    logger.fatal("fatal level");  
}  
```



log4j2.xml

```xml
<Configuration status="WARN" monitorInterval="300">  
    <Appenders>  
        <Console name="Console" target="SYSTEM_OUT">  
            <PatternLayout pattern="%d{HH:mm:ss.SSS} [%t] %-5level %logger{36} - %msg%n" />  
        </Console>  
    </Appenders>  
    <Loggers>  
        <Logger name="mylog" level="trace" additivity="false">  
        <AppenderRef ref="Console" />  
    </Logger>  
        <Root level="error">  
            <AppenderRef ref="Console" />  
        </Root>  
    </Loggers>  
</Configuration>
```

getLogger("mylog") 获取了 `name="mylog"` 的 Logger, additivity 属性表示是否延伸到父层的 Logger, 如果改为true, 那么 Root Logger 也会触发一次

根节点的 monitorInterval="300" 代表每隔300秒重新读取配置文件, 这样可以不重启应用的情况下修改配置



# 自定义Appender

log4j2.xml

```xml
<Configuration status="WARN" monitorInterval="300">  
    <Appenders>  
        <Console name="Console" target="SYSTEM_OUT">  
            <PatternLayout pattern="%d{HH:mm:ss.SSS} [%t] %-5level %logger{36} - %msg%n" />  
        </Console>  
        <File name="MyFile" fileName="/home/frank/log/mylog.log">  
            <PatternLayout pattern="%d{HH:mm:ss.SSS} [%t] %-5level %logger{36} - %msg%n" />  
        </File>  
    </Appenders>  
    <Loggers>  
        <Logger name="mylog" level="trace" additivity="true">  
            <AppenderRef ref="MyFile" />  
        </Logger>  
        <Root level="error">  
            <AppenderRef ref="Console" />  
        </Root>  
    </Loggers>  
</Configuration>  
```

如果使用 mylog 的 Logger 会将日志信息追加到 /home/frank/log/mylog.log 中



# 实用性配置

log4j2.xml

```xml
<Configuration status="WARN" monitorInterval="300">  
    <properties>  
        <property name="LOG_HOME">/home/frank/log</property>  
        <property name="FILE_NAME">mylog</property>  
    </properties>  
    <Appenders>  
        <Console name="Console" target="SYSTEM_OUT">  
            <PatternLayout pattern="%d{HH:mm:ss.SSS} [%t] %-5level %logger{36} - %msg%n" />  
        </Console>  
        <RollingRandomAccessFile name="MyFile"  
            fileName="${LOG_HOME}/${FILE_NAME}.log"  
            filePattern="${LOG_HOME}/$${date:yyyy-MM}/${FILE_NAME}-%d{yyyy-MM-dd HH-mm}-%i.log">  
            <PatternLayout  
                pattern="%d{yyyy-MM-dd HH:mm:ss.SSS} [%t] %-5level %logger{36} - %msg%n" />  
            <Policies>  
                <TimeBasedTriggeringPolicy interval="1" />  
                <SizeBasedTriggeringPolicy size="10 MB" />  
            </Policies>  
            <DefaultRolloverStrategy max="20" />  
        </RollingRandomAccessFile>  
    </Appenders>  
  
    <Loggers>  
        <Logger name="mylog" level="trace" additivity="false">  
            <AppenderRef ref="MyFile" />  
        </Logger>  
        <Root level="error">  
            <AppenderRef ref="Console" />  
        </Root>  
    </Loggers>  
</Configuration>
```

配置按时间和文件大小滚动的 RollingRandomAccessFile Appender, 当满足一定条件的时候就会重命名原日志文件, 然后重新生成一个日志文件. 例如配置1天生成一个日志文件, 而且如果日志文件超过10 MB就重新生成日志文件

properties 配置的变量方便以后修改

RollingRandomAccessFile 的属性:

- fileName: 当前日志文件的位置和文件名
- filePattern: 当发生Rolling时, 文件的转移和重命名规则
- SizeBasedTriggeringPolicy: 当文件大于size时, 触发Rolling
- DefaultRolloverStrategy: 指定最多保存个文件个数
- TimeBasedTriggeringPolicy: 需要配置filePattern结合使用, ${FILE_NAME}-%d{yyyy-MM-dd HH-mm} 最小时间粒度是分钟, 而TimeBasedTriggeringPolicy 的 interval 是1, 所以是每分钟生成一个新文件, 如果filePattern改成%d{yyyy-MM-dd HH} 最小粒度是小时, 那么就是一个小时生成一个文件



# 自定义配置文件配置

log4j2 默认是在 classpath 下找配置文件, 在非web项目通过下面配置配置文件

```java
public static void main(String[] args) throws IOException {  
    File file = new File("/home/frank/log4j2.xml");  
    BufferedInputStream in = new BufferedInputStream(new FileInputStream(file));  
    final ConfigurationSource source = new ConfigurationSource(in);  
    Configurator.initialize(null, source);  
      
    Logger logger = LogManager.getLogger("mylog");  
}  
```

Web项目可以在web.xml中配置

```xml
<context-param>  
    <param-name>log4jConfiguration</param-name>  
    <param-value>/WEB-INF/conf/log4j2.xml</param-value>  
</context-param>  
  
<listener>  
    <listener-class>org.apache.logging.log4j.web.Log4jServletContextListener</listener-class>  
</listener>
```

