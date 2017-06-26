---
layout: post
title: "Jasper Reports, HTML and Pagination"
date: 2011-03-02 18:43
categories: java jasper
---

In one of my previous posts, I had mentioned how to ignore pagination in excel reports.
To achieve the same in HTML reports , we need to set the following parameters to the exporter before exporting.

```java
htmlExporter.setParameter(JRHtmlExporterParameter.BETWEEN_PAGES_HTML, "");
htmlExporter.setParameter(JRHtmlExporterParameter.HTML_HEADER, "");
htmlExporter.setParameter(JRHtmlExporterParameter.HTML_FOOTER, "");
htmlExporter.setParameter(JRHtmlExporterParameter.IS_REMOVE_EMPTY_SPACE_BETWEEN_ROWS,
                    Boolean.TRUE);
```

For more details , you can refer to [this](http://jasperforge.org/uploads/publish/jasperreportswebsite/trunk/sample.reference/nopagebreak/index.html) page.
