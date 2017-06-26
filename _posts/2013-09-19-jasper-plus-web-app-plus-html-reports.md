---
layout: post
title: "Jasper + Web App + HTML Reports"
date: 2011-01-20 18:24
comments: true
categories: java jasper
---

I had lots of difficulties in getting HTML reports to work in my web application. When I added a couple of images in the report, I could see them when exporting the report to PDF, but could not see the images when exported to HTML. All I could see was a bunch of broken images !Here is how to solve this issue.

In the web.xml file add the following entry:

```xml
<servlet>
    <servlet-name>ImageServlet</servlet-name>
    <servlet-class>net.sf.jasperreports.j2ee.servlets.ImageServlet</servlet-class>
</servlet>
 
<servlet-mapping>
    <servlet-name>ImageServlet</servlet-name>
    <url-pattern>/servlets/image</url-pattern>
</servlet-mapping>
```

The part in your code where you export the report to HTML should look like this.

```java
JRHtmlExporter htmlExporter = new JRHtmlExporter();
 
// without the below line , images will not appear in the HTML report
session.setAttribute(ImageServlet.DEFAULT_JASPER_PRINT_SESSION_ATTRIBUTE,
            filledReport);
htmlExporter.setParameter(JRExporterParameter.JASPER_PRINT,
            filledReport);
htmlExporter.setParameter(JRExporterParameter.OUTPUT_STREAM, baos);
htmlExporter.setParameter(JRHtmlExporterParameter.IMAGES_URI,
            "../servlets/image?image=");
htmlExporter.exportReport();
```

Make sure that you set the path to the image servlet right in the JRHtmlExporterParameter.IMAGES_URI parameter.
As mention in the code comments, the most important step is to set the ImageServlet.DEFAULT_JASPER_PRINT_SESSION_ATTRIBUTE in the session to the JasperPrint object. If you do not set this, you will see the following exception when you access /servlets/image in the browser:

```
javax.servlet.ServletException: No JasperPrint documents found on the HTTP session.
at net.sf.jasperreports.j2ee.servlets.ImageServlet.se rvice(ImageServlet.java:106)
at javax.servlet.http.HttpServlet.service(HttpServlet .java:810)
```

Since thie image uri is giving an exception, you will see broken images in the HTML.
So, it is mandatory to set the attribute ImageServlet.DEFAULT_JASPER_PRINT_SESSION_ATTRIBUTE to the JasperPrint object in the session.
