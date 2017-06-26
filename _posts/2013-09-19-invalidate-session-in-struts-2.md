---
layout: post
title: "Invalidate Session in Struts 2"
date: 2011-01-25 17:32
comments: true
categories: java struts2
---

To access the session object in your action class, it should implement the interface : org.apache.struts2.interceptor.SessionAware.

Refer to this for more details on the SessionAware interface.
Once you have access to the session object, add the below piece of code to invalidate the session (typically used for logout mechanism in webapps).

``` java 
if (session instanceof org.apache.struts2.dispatcher.SessionMap) {
    try {
        ((org.apache.struts2.dispatcher.SessionMap) session).invalidate();
    } catch (IllegalStateException e) {
        logger.error(msg, e);
    }
}
```
