---
layout: post
title: Integrating HornetQ with Camel and Spring
categories:
- J2EE
- Java
- JBoss
tags:
- Camel
- HornetQ
- J2ee
- Java
- JBoss
- JMS
status: publish
type: post
published: true
comments: true
meta:
  _edit_last: '2'
  _wpas_skip_2592522: '1'
---
Since [Apache Camel](http://camel.apache.org/) have only one built-in implementation for specific JMS provider - [ActiveMQ](http://activemq.apache.org/), you have to use generic `org.apache.camel.component.jms.JmsConfiguration`.
In order to work with [HornetQ](http://www.jboss.org/hornetq) using Camel and Spring first you have to add HornetQ dependencies to Maven pom.xml:
``` xml pom.xml
<dependency>
	<groupId>org.hornetq</groupId>
	<artifactId>hornetq-jms</artifactId>
	<version>${hornetq.version}</version>
</dependency>
<dependency>
	<groupId>org.hornetq</groupId>
	<artifactId>hornetq-jms-client</artifactId>
	<version>${hornetq.version}</version>
</dependency>
<dependency>
	<groupId>org.hornetq</groupId>
	<artifactId>hornetq-core-client</artifactId>
	<version>${hornetq.version}</version>
</dependency>
<dependency>
	<groupId>org.jboss.netty</groupId>
	<artifactId>netty</artifactId>
	<version>3.2.7.Final</version>
</dependency>
```
After that modify your spring.xml. Here is an example:
<!--more-->
``` xml spring.xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xsi:schemaLocation="http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans-3.0.xsd">
    <bean id="hornetqServerLocatorFactory" class="com.example.HornetqServerLocatorFactory" />
    <bean id="hornetqServerLocator" factory-bean="hornetqServerLocatorFactory" factory-method="createServerLocator"/>
    <bean id="hornetqJmsConnectionFactory" class="org.hornetq.jms.client.HornetQJMSConnectionFactory">
        <constructor-arg name="serverLocator" ref="hornetqServerLocator"/>
    </bean>
    <bean id="hornetqJmsConfig" class="org.apache.camel.component.jms.JmsConfiguration">
        <property name="connectionFactory" ref="hornetqJmsConnectionFactory"/>
        <property name="transacted" value="false"/>
        <property name="requestTimeout" value="60000"/>
    </bean>
    <bean id="hornetq" class="org.apache.camel.component.jms.JmsComponent">
        <property name="configuration" ref="hornetqJmsConfig"/>
    </bean>
</beans>
```
And the last thing is to write your implementation of `HornetqServerLocatorFactory`. If you wish to configure HornetQ host and port from spring.xml, you can easily do it - just put them as members of the class and inject them via Spring.xml.
``` java HornetqServerLocatorFactory.java
	package com.example;

	import java.util.HashMap;
	import java.util.Map;
	import org.hornetq.api.core.TransportConfiguration;
	import org.hornetq.api.core.client.HornetQClient;
	import org.hornetq.api.core.client.ServerLocator;
	import org.hornetq.core.remoting.impl.netty.NettyConnectorFactory;
	import org.springframework.beans.factory.annotation.Configurable;

	@Configurable
	public class HornetqServerLocatorFactory {

		public ServerLocator createServerLocator() {
			final Map<String, Object> connectionParams = new HashMap<String, Object>();
			connectionParams.put( "port", "hornetq-host.com" );
			connectionParams.put( "host", 5900 );

			final TransportConfiguration transportConfiguration = new TransportConfiguration( NettyConnectorFactory.class.getName(), connectionParams );
			final ServerLocator serverLocator = HornetQClient.createServerLocatorWithoutHA( new TransportConfiguration[] { transportConfiguration } );
			return serverLocator;
		}
	}
```
That's it
