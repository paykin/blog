---
layout: post
title: Liferay portlets deployment in IntelliJ IDEA
categories:
- Java
tags:
- IntelliJ IDEA
- Java
- liferay
- portlets
status: publish
type: post
published: true
comments: true
meta:
  _edit_last: '2'
  _wpas_done_all: '1'
  _wpas_skip_2592522: '1'
---
You can develop Liferay portlets with IntelliJ IDEA exactly as in Eclipse Liferay IDE. Follow simple steps:
<ol>
	<li><span style="line-height: 14px;">Create debug configuration in IntelliJ IDEA using Local Tomcat</span></li>
	<li>Configure deployment artifact: add your WAR as <code>yourwar:war exploded</code> and set <code>Application context</code> to <code>/yourwar</code></li>
	<li>Create XML file with name "yourwar.xml" with the following content:
``` xml yourwar.xml
<?xml version="1.0" encoding="UTF-8"?>
<Context 
  docBase="/absolute/path/to/project/yourwar" 
  reloadable="false" 
  path="/yourwar"/>
```

Please note, docBase have to be the same as "Output directory" setting in ItelliJ IDEA of the artifact you deploy. Also, path have to be the same as "Application context" in Run/Debug configuration</li>
	<li>Create post deployment action that will copy just created XML to Liferay's autodeploy folder.</li>
  <li>As <a href="http://www.liferay.com/community/wiki/-/wiki/Main/Running+Liferay+from+IntelliJ" title="Liferay official wiki" target="_blank">Liferay official wiki</a> suggests, add empty <code>CATALINA_BASE</code> environment variable.</li>
</ol>

Enjoy automatic deployment, hot-swap and debugging your LIferay application.



Please note, I tested it with IntelliJ IDEA 12.1 and Liferay 6.0.5 on Tomcat 6
