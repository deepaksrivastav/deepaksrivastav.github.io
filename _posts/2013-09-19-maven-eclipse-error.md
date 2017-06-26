---
layout: post
title: "Resolve Maven Eclipse Plugin execution not covered by lifecycle configuration error"
date: 2012-07-05 19:05
comments: true
categories: eclipse maven
---

Posting this after a very long time. But I believe this is pretty useful.

Recently, when I imported a Maven project in eclipse, it started giving two errors which more or less looked as follows:

Plugin execution not covered by lifecycle configuration: org.apache.maven.plugins:maven-resources-plugin:2.4.3:resources (execution: default-resources, phase: process-resources)….

Plugin execution not covered by lifecycle configuration: org.apache.maven.plugins:maven-resources-plugin:2.4.3:testResources (execution: default-testResources, phase: process-test-resources)	…


I was using the M2E Plugin for Eclipse Maven Integration and read the following article to find a fix for the errors:
http://wiki.eclipse.org/M2E_plugin_execution_not_covered

Just to save some reading time, i just included the below piece of code in the pom.xml file and the errors vanished !

```xml
<build>
    <pluginManagement>
        <plugins>
            <!-- Ignore/Execute plugin execution -->
            <!-- this is to eliminate eclipse import errors -->
            <plugin>
                <groupId>org.eclipse.m2e</groupId>
                <artifactId>lifecycle-mapping</artifactId>
                <version>1.0.0</version>
                <configuration>
                    <lifecycleMappingMetadata>
                        <pluginExecutions>
                        <!-- copy-dependency plugin -->
                            <pluginExecution>
                                <pluginExecutionFilter>
                                    <groupId>org.apache.maven.plugins</groupId>
                                    <artifactId>maven-resources-plugin</artifactId>
                                    <versionRange>[2.4.3,)</versionRange>
                                    <goals>
                                        <goal>resources</goal>
                                        <goal>testResources</goal>
                                    </goals>
                                </pluginExecutionFilter>
                                <action>
                                    <ignore />
                                </action>
                            </pluginExecution>
                        </pluginExecutions>
                    </lifecycleMappingMetadata>
                </configuration>
             </plugin>
        </plugins>
    </pluginManagement>
</build>
```

