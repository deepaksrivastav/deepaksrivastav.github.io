---
layout: post
title: "Accessing Context, Session, Request and Response Objects in the Struts2 Action Class"
date: 2011-01-25 22:27
comments: true
categories: java struts2
---

To get access to the Servlet Context object in your action class, your action class (apart from extending ActionSupport) should also implement ServletContextAware interface (org.apache.struts2.util.ServletContextAware). The action class should have the ServletContextVariable and should implement the void setServletContext(ServletContext ctxt) method. Take a look at the below example for more clarity.

```java
package com.struts.sample;
 
import javax.servlet.ServletContext;
 
import org.apache.struts2.util.ServletContextAware;
 
import com.opensymphony.xwork2.ActionSupport;
 
public class TestAction extends ActionSupport implements ServletContextAware {
 
    private ServletContext context = null;
 
    @Override
    public void setServletContext(ServletContext context) {
        this.context = context;
    }
 
}
```

Similarly, to get access to Session object you need to implement the SessionAware (org.apache.struts2.interceptor.SessionAware). To get access to Request and response objects, implement the org.apache.struts2.interceptor.ServletRequestAware and org.apache.struts2.interceptor.ServletResponseAware interfaces respectively.

Now, what if we want to access all these objects in all our action classes?

In that case, all we need to do is create a base class which implements the required interfaces mentioned above and have all your action classes extend this base class.

A sample implementation of BaseActionSupport is as mentioned below :

```java
package com.nlt.reporting.action;
 
import java.util.Map;
 
import javax.servlet.ServletContext;
import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;
 
import org.apache.struts2.interceptor.ServletRequestAware;
import org.apache.struts2.interceptor.ServletResponseAware;
import org.apache.struts2.interceptor.Sessiouts2 ActionSupport. (If you notice that in the above example, the instance variables of BaseAction is protected. This is to make sure that the objects that extend this class will have access to the instance variables of the BaseAction class)nAware;
import org.apache.struts2.util.ServletContextAware;
 
import com.opensymphony.xwork2.ActionSupport;
 
public class BaseAction extends ActionSupport implements ServletContextAware,
        SessionAware, ServletRequestAware, ServletResponseAware {
 
    protected ServletContext context = null;
    protected Map<String, Object> session = null;
    protected HttpServletRequest request = null;
    protected HttpServletResponse response = null;
 
    @Override
    public void setServletContext(ServletContext context) {
        this.context = context;
    }
 
    @Override
    public void setSession(Map<String, Object> session) {
        this.session = session;
 
    }
 
    @Override
    public void setServletRequest(HttpServletRequest request) {
        this.request = request;
    }
 
    @Override
    public void setServletResponse(HttpServletResponse response) {
        this.response = response;
    }
 
}
```

Now all you need to do when you are creating you action class is to extend this BaseAction instead of the struts2 ActionSupport. (If you notice that in the above example, the instance variables of BaseAction is protected. This is to make sure that the objects that extend this class will have access to the instance variables of the BaseAction class)

