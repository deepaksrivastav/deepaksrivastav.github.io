---
layout: post
title: "A Web Reporting Tool"
date: 2011-01-03 18:07
categories: java mybatis struts2 jasper
---

Recently I was given an assignment to develop a web based reporting tool. It needed the following features:

1. SQL queries given by a Data Architect.
2. Need a web interface to format the result of that query.
3. Should be able to export the report into PDF / Excel / CSV / HTML.

I started searching for tools which can be used and came up with the following list :

1. Jasper Reports (including JFreeCharts) for reporting and creating charts.
2. iReport for designing reports.
3. Struts 2 (MVC)

The “one” question that remained in my mind was how to fetch the data for the reports. Jasper has its own method where-in, if you pass a JDBC Connection, it will execute the query written in the report itself and format the fields. But this was not very flexible for us as there was some business logic to be executed after the query was run.

Jasper Reports can take data in the form of a collection (a collection of beans). So Hibernate was the clear choice. But for us to use hibernate, we need to know the schema well to create mappings. The database is very complex and would take some considerable amount of time to get the hibernate mappings right.

I came across an article in java world about using iBATIS for feeding data to the reports. iBATIS is now MyBatis as has been moved to google code. MyBatis is basically an object mapper framework which maps objects with SQL queries or Stored Procedures using an XML descriptor. We also found out that MyBatis is well documented as well. It has a very good user guide and has some goodies like Guice and Spring integration.

Check out the site : http://code.google.com/p/mybatis/.

You can read more about Jasper and iBATIS [here](http://www.javaworld.com/javaworld/jw-12-2007/jw-12-ibatisjasper.html)
