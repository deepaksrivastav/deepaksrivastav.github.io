---
layout: post
title: "Formatted Date in Struts 2 Text Field"
categories: java struts2
---

There is a simple way to have a formatted date in the text input (textfield) of struts 2.
Assuming that we have a instance variable “currentDate” of type Date in our action class.
You can populate the text box in your JSP after formatting the date as shown below :

```html
<s:date name="currentDate" var="formattedDate" format="dd-MM-yyyy"/>

<s:textfield name="enterDate" value="%{formattedDate}"/>
```
