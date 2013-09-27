---
layout: post
title: Enums in JSF
categories:
- JSF
tags:
- Java
- JSF
status: publish
type: post
published: true
comments: true
meta:
  _edit_last: '2'
---
Finally I find the way to use enum with JSF:
Suppose you have enum:
``` java
public enum ActionTypeEnum {
	UPDATE,
	CREATE,
	COPY
}
```
In JSF you can write EL expressions in form:
``` xml
<h:outputText value="someText"
	rendered="#{listAction.actionType == 'COPY'}"/>
```	
The property <code>actionType</code> in managed bean <code>listAction</code> have to be of type <code>ActionTypeEnum</code>.
It works because each enum in have method <code>valueOf(String stringValue)</code> and JSF attempts to convert String value to enum.
