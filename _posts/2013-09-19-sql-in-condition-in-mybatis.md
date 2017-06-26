---
layout: post
title: "SQL 'IN' Condition in MyBatis"
date: 2011-02-11 18:38
categories: java sql mybatis
---
You will often need to build an SQL query with an ‘in’ condition. By this we mean to build the SQL query dynamically.

MyBatis is very powerful when it comes to building Dynamic SQL queries ans let us see how to achieve the above with the same.
For example, assume that our query is :

```sql
select * from my_data where create_date in (’2011-01-01′,’2010-01-01′);
```

We can use ‘foreach’ expression in the select query and the mapper xml would have an entry that looks like this:

```xml
<select id="getMyDataOnDates" resultType="domain.obj.MyData" parameterType="list">
SELECT *
FROM my_data
WHERE create_date in
    <foreach item="item" index="index" collection="list"
       open="(" separator="," close=")">
         #{item}
     </foreach>
</select>
```

The method in the report mapper interface would look like this :

```java
List<MyData> getMyDataOnDates(List<Date> dateList);
```

Not that the parameter type passed should be either a list or array.List instances will be keyed to the name “list” and array instances will be keyed to the name “array”.
