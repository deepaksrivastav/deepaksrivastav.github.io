---
layout: post
title: "Disable Struts 2 Log Messages"
date: 2011-01-06 18:15
categories: java struts2 log4j
---

I was literally struggling to debug using log messages as it was cluttered with logs from Struts 2 / Xworks and freemarker.
Took some time to dig into log4j an got a solution finally !

Log4j has the ability to set different log levels for different packages and hence using the log level OFF we can disable logging for a particular package.
In case the struts 2, the debug messages mostly come from the following packages:

```
com.opensymphony.xwork2
org.apache.struts2
freemarker.cache
freemarker.beans
```

To disable logging from these packages, use the following in your log4j.xml after the appender section.

```xml
<logger name="com.opensymphony.xwork2">
    <level value="OFF" />
</logger>

<logger name="freemarker.cache">
    <level value="OFF" />
</logger>

<logger name="freemarker.beans">
    <level value="OFF" />
</logger>

<logger name="org.apache.struts2">
    <level value="OFF" />
</logger>
```

My xml looks something like this :

```xml
<?xml version="1.0" encoding="UTF-8" ?>
<!DOCTYPE log4j:configuration SYSTEM "log4j.dtd">
<log4j:configuration xmlns:log4j='http://jakarta.apache.org/log4j/'>
    <appender name="FA" class="org.apache.log4j.FileAppender">
        <param name="File" value="sample.log" />
        <param name="Threshold" value="DEBUG" />
        <layout class="org.apache.log4j.PatternLayout">
            <param name="ConversionPattern" value="%-4r [%t] %-5p %c %x - %m%n" />
        </layout>
    </appender>

    <logger name="com.opensymphony.xwork2">
        <level value="OFF" />
    </logger>

        <logger name="org.apache.struts2">
        <level value="OFF" />
    </logger>

    <logger name="freemarker.cache">
        <level value="OFF" />
    </logger>

    <logger name="freemarker.beans">
        <level value="OFF" />
    </logger>

    <logger name="org.apache.ibatis">
        <level value="OFF" />
    </logger>

    <root>
        <level value="DEBUG" />
        <appender-ref ref="FA" />
    </root>
</log4j:configuration>
```

As you can see, I have turned off messages from ibatis as well.
If you are not using xml for Log4j configuration and using a resource file(properties file).
You can achieve the same using the following in log4j.properties (or whatever file you have chosen as log4j resource file)

```
log4j.category.com.opensymphony.xwork2=OFF
log4j.category.org.apache.struts2=OFF
log4j.category.freemarker.beans=OFF
log4j.category.freemarker.cache=OFF
```
