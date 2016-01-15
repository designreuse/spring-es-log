# spring-es-log
Integration of ElasticSearch-Log Appender in spring-boot without xml configuration

bases on https://github.com/internetitem/logback-elasticsearch-appender

motivation : use elasticsearch-appender without logback.xml 
cofiguration in appication.yml of the spring boot container


application.yml exmpl:

```yml
server:
  port: 8080                                       # is required if you use parameters.port
   
spring:
  application:
    name: myApp                                    # is required if you use parameters.application
    log: 
      enable-es-log: true                          # default is true
      es-log-url: "http://localhost:9200/_bulk"    # default is localhost:9200/_bulk
      index-name: "log-%date{yyyy-MM-dd}"          # default is log-%date{yyyy-MM-dd} 
      type: "eslog"                                # default is eslog
      parameters:        
        severity: "%level"                         # default is %level
        thread: "%thread"                          # default is %thread
        logger: "%logger"                          # default is %logger
        stacktrace: "%ex"                          # default is %ex
        host: "%X{host}"                           # optional will print name of the host
        port: "%X{port}"                           # optional will print server.port if set
        application: "%X{spring.application.name}" # optional will print spring.application.name
        
```
usage:
simply add it in your maven pom 
```xml
		<dependency>
			<groupId>de.db0x</groupId>
			<artifactId>spring-es-log</artifactId>
			<version>0.0.1</version>
		</dependency>
```
and annotate an class with @EnableESLog.

pro + no xml configuration is needed
neg - the first events are not logged to ES because the appender is armed after the spring-context is loaded 