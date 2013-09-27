---
layout: post
title: Forcing JBoss 7 to apply changes to JSP's immediately
categories:
- J2EE
- Java
- JBoss
- JSF
tags:
- J2ee
- Java
- JBoss
- JSF
status: publish
type: post
published: true
comments: true
meta:
  _wp_old_slug: jboss-7-changes-js
---
JBoss AS 7 by default do not reflect any changes to JSP files in WAR deployment. So, during application development you have to redeploy WAR or restart JBoss. In order to change this behaviour it is necessary to change section of `urn:jboss:domain:web:1.0` in `standalone.xml` so it looks like:
``` xml standalone.xml
    <configuration>
        <jsp-configuration development="true"/>
    </configuration>
```
And now JBoss behaves as you expect it in development mode: all changes to JSP files are loaded automatically as soon as you change the JSP file.
