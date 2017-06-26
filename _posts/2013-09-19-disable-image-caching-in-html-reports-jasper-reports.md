---
layout: post
title: "Disable Image Caching in HTML Reports(Jasper Reports)"
date: 2011-03-03 18:45
categories: java jasper
---

This is the most annoying thing that can happen to a web developer. If you have a graph / chart in your report and you are rendering the same in your web application (refer here), you might have noticed that the graph does not refresh most of the times in IE and sometimes in Chrome/Firefox.

Of course , to get the graph to work, you need to follow the instructions as mentioned in my previous blog here.

Remember that to render any image in the HTML reports, you need to define an ImageServlet of type net.sf.jasperreports.j2ee.servlets.ImageServlet in web.xml and set the same to the JRHtmlExporter before exporting.

To disable cache, a simple solution is to append a random number to the request string while setting the JRHtmlExporterParameter.IMAGES_URI parameter of JRHtmlExporter.

The code is as follows:

```java
htmlExporter.setParameter(JRHtmlExporterParameter.IMAGES_URI,
    "servlets/image?rnd=" + Math.random() + "&image=");
```
