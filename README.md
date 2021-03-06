# spring-es-log
Integration of ElasticSearch-Log Appender in spring-boot without xml configuration

[![Build Status](https://travis-ci.org/db0x/spring-es-log.svg?branch=master)](https://travis-ci.org/db0x/spring-es-log)

bases on https://github.com/internetitem/logback-elasticsearch-appender

stable : 0.0.9 (spring-boot < 1.4 and es < 2.0 )
         0.1.0 (spring-boot = 1.4 and es > 2.0 )

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
      host: localhost                              # host of the es cluster default is localhost
      ports: [9200,9300]                           # ports of the es cluster 
                                                   #   default is 9200 / 9300
                                                   #   (HTTP / transport)
      clustername: elasticsearch			       # cluster name of the ES cluster
      											   #   default is elasticsearch
      type: "eslog"                                # _type in index will be used for clean-query
      index-name: "log-%date{yyyy-MM-dd}"          # pattern for index-name
                                                   #   default is log-%date{yyyy-MM-dd}
      clean: 5                                     # indices will be cleaned after x days
                                                   #   (-1 -> never clean indices) default is 5                 
      clean-interval: 60                           # interval of clean in minutes default is 60
      clean-number-of-documents: 100000            # number of documents will be deleted 
                                                   #   in one run default is 10000 
      parameters:        
        severity: "%level"                         # default is %level
        thread: "%thread"                          # default is %thread
        logger: "%logger"                          # default is %logger
        stacktrace: "%ex"                          # default is %ex
        host: "%X{host}"                           # optional will print name of the host
        port: "%X{port}"                           # optional will print server.port if set
        application: "%X{spring.application.name}" # optional will print spring.application.name
        
```
Usage
=====
- clone this repo
- build it
- use it

or
- add this to your maven pom 

```xml
<repositories>
	<repository>
		<id>db0x</id>
		<url>http://mvn.db0x.de</url>
	</repository>
</repositories>
<dependencies>
    <dependency>
    	<groupId>de.db0x</groupId>
	    <artifactId>spring-es-log</artifactId>
	    <version>0.0.7</version>
    </dependency>
</dependencies>
```

and annotate an class with @EnableESLog.

* pro + no xml configuration is needed
* neg - the first events are not logged to ES because the appender is armed after the spring-context is loaded

Usage of additional %X{...} parameters
===================================
you can add new columns to the index using parameter and %X{...}

with %X you can read what was put into org.slf4j.MDC

0.1.0 -> changed to springboot 1.4 + es 2.3.4

0.0.9 -> upgrade to logback-elasticsearch-appender 1.3
0.0.8 -> was not longer compatible to logback-elasticsearch-appender
0.0.7 -> version-madness 
0.0.6 -> still version-problems
      -> + TestSuite
0.0.5 -> change of min-versions of Spring / Spring-Boot / ES
---


