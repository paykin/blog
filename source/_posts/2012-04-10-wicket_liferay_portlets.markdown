---
layout: post
title: Portlets with Wicket 1.5.x in Liferay portal
categories:
- Wicket
tags:
- liferay
- portlet
- wicket
status: publish
type: post
published: true
comments: true
meta:
  _edit_last: '2'
  _wp_old_slug: wicket_liferay_portlet
  _wpas_skip_2592522: '1'
---
After Wicket dropped support for portlets and moving it to wicketstuff the settings in XMLs are changed:

portlet.xml should look like
``` xml
<portlet>
	<description>wickettest4</description>
	<portlet-name>wickettest4</portlet-name>
	<display-name>wickettest4</display-name>
	<portlet-class>org.apache.wicket.portlet.WicketPortlet</portlet-class>
	<init-param>
		<name>wicketFilterPath</name>
		<value>/bla</value>
	</init-param>
	<supports>
		<mime-type>text/html</mime-type>
		<portlet-mode>VIEW</portlet-mode>
	</supports>
	<portlet-info>
		<title>wickettest4</title>
		<keywords>wickettest4</keywords>
	</portlet-info>
</portlet>
```

Note new portlet-class. Also, mime-type have to be text/html.
<!--more-->
web.xml also changed, notice filter class:
``` xml
<filter>
	<filter-name>wicket.wicket</filter-name>
	<filter-class>org.apache.wicket.portlet.PortletFilter</filter-class>
	<init-param>
		<param-name>applicationClassName</param-name>
		<param-value>org.wicket.WicketApplication</param-value>
	</init-param>
</filter>
<filter-mapping>
	<filter-name>wicket.wicket</filter-name>
	<url-pattern>/bla/*</url-pattern>
	<dispatcher>REQUEST</dispatcher>
	<dispatcher>INCLUDE</dispatcher>
	<dispatcher>FORWARD</dispatcher>
</filter-mapping>
```

And of course Maven dependencies also changed:
``` xml
<dependency>
	<groupId>org.apache.wicket</groupId>
	<artifactId>wicket-core</artifactId>
	<version>${wicket.version}</version>
</dependency>
<dependency>
	<groupId>org.apache.portals.bridges</groupId>
	<artifactId>portals-bridges-common</artifactId>
	<version>2.0</version>
</dependency>
<dependency>
	<groupId>org.wicketstuff</groupId>
	<artifactId>wicketstuff-portlet</artifactId>
	<version>${wicket.version}</version>
</dependency>
```
