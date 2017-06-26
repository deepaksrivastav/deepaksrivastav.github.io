---
layout: post
title: "Creating a Struts2 project with Maven"
date: 2011-01-03 17:54
comments: true
categories: java struts2 maven
---

Here is how to create a struts 2 project skeleton using Struts 2 Archetypes for Maven 2.
Struts 2 provides about three maven archetypes, namely:

1. Starter (struts2-archetype-starter)
2. Portlet Blank (struts2-archetype-portlet)
3. Portlet database (struts2-archetype-dbportlet)

The most commonly used among this is the starter type.

To create a project of the struts 2 starter archetype, use the following command:

```
$ mvn archetype:generate -B -DgroupId=com.struts.test -DartifactId=testProject -DarchetypeGroupId=org.apache.struts -DarchetypeArtifactId=struts2-archetype-blank -DarchetypeVersion=2.2.1
```

It will take a while to create the project if you are creating it for the first time after installing maven. The reason is that maven will download all the dependencies (libraries) from the internet. Note that the maven repository (all the dependencies) are downloaded in the following directory in case of windows (C:\Documents and Settings\\.m2\repository). Just make a note of this for now.

When the creation is complete, the directory would look as shown below :

{% img images/mvnStrutsDirStructure.png %}

If you have used struts 2 before you would quickly recognize that this is a struts2 blank war that we used to start off the project.
This project does not by default mention the source and the target version of Java you are intending to use. It is always a good practice to mention the source and target. To do this , add the following to the section of the “pom.xml” file in the project directory.

``` xml
<plugin>
    <groupId>org.apache.maven.plugins</groupId>
    <artifactId>maven-compiler-plugin</artifactId>
    <version>2.3.2</version>
    <configuration>
        <source>1.6</source>
        <target>1.6</target>
    </configuration>
 </plugin>
```

If you have not followed the above step, you may later encounter an error after importing the project into eclipse. The error would look like this : “Java compiler level does not match the version of the installed Java project facet”.
Now , create an eclipse project using the following command.

```
mvn eclipse:eclipse 
```

This would create the eclipse project files in the same directory and you can ‘import’ this project in Eclipse.
Once you import it, you would see some errors regarding the missing jar files in the M2_REPO class path.
To solve this, you need to create a classpath variable in eclipse. The procedure to do the same is as follows :

1. Right click on the project and select Properties.
2. Select Java Build Path in the left navigation panel.
3. Select Libraries tab in the right panel.
4. Click on Add Variable and a popup appears.
5. Click on “Configure Variables…” and this again brings up another popup dialog.
6. Click on New
7. Enter variable name as M2_REPO and the path will be the maven repository path that I mention before (ideally C:\Documents and Settings\\.m2\repository in windows).
8. Click on OK and return and Refresh the project.

Now your eclipse project setup is ready. Delete all the sample files that are installed by default and start adding your implementation.
To build the project, use the command ```mvn install```

To cleanup the project, use ```mvn clean```

To run the test cases, use ```mvn test```

To package the project ```mvn package```

Just make sure that you execute all the commands from project root directory
The project war file will be present in the target directory created in the project directory after successful build of the project.
You can find the official documentation for Struts 2 Maven Archetypes [here](http://struts.apache.org/release/2.0.x/docs/struts-maven-archetypes.html) .

