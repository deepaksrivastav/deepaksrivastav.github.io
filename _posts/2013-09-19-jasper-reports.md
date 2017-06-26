---
layout: post
title: "Jasper Reports, Excel and Pagination"
categories: java jasper
---

My project wanted me to have reports which has pagination enabled for PDF and HTML formats but not for the Excel Reports. I had originally achieved it by having different report templates for PDF/HTML and for XLS. But I knew I would soon run into problems as this solution is not maintainable. I wanted to have one JRXML file and while exporting the report as excel file, pageHeader and pageFooter sections should be excluded and the columnHeader section should only be printed once for the entire report.

This can be done using certain properties in the JRXML file. I am listing the properties I used to get an optimal excel export. You can find more on these properties in the Config Reference section in Jasperforge ([link](http://jasperforge.org/uploads/publish/jasperreportswebsite/trunk/config.reference.html)):

```xml
<property name="net.sf.jasperreports.export.xls.exclude.origin.band.1" value="pageHeader"/>
<property name="net.sf.jasperreports.export.xls.exclude.origin.band.2" value="pageFooter"/>
<property name="net.sf.jasperreports.export.xls.exclude.origin.keep.first.band.1" value="columnHeader"/>
<property name="net.sf.jasperreports.export.xls.remove.empty.space.between.rows" value="true"/>
<property name="net.sf.jasperreports.export.xls.remove.empty.space.between.columns" value="true"/>
```
