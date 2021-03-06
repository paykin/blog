---
layout: post
title: Create datasource programmatically on JBoss 7
categories:
- J2EE
- Java
- JBoss
tags:
- J2ee
- Java
- JBoss
status: publish
type: post
published: true
comments: true
meta:
  _edit_last: '2'
  _wpas_skip_2592522: '1'
---
In order to create new datasource programmatically on JBoss 7 without restart (on the fly) from Java you can use CLI Java API. You have to include jboss-as-controller-client to your project dependencies:
``` xml 
<dependency>
	<groupId>org.jboss.as</groupId>
	<artifactId>jboss-as-controller-client</artifactId>
	<version>7.0.2.Final</version>
</dependency>
```
The following example creates new datasource:
<!--more-->
``` java 
	public void createDatasource() throws Exception{
		ModelNode request = new ModelNode();
		request.get(ClientConstants.OP).set(ClientConstants.ADD);
		request.get(ClientConstants.OP_ADDR).add("subsystem",
				"datasources");
		request.get(ClientConstants.OP_ADDR).add("data-source",
				"java:jboss/datasources/NewDatasource");
		request.get("jndi-name").set("java:jboss/datasources/NewDatasource");
		request.get("connection-url").set("jdbc:as400://1.2.3.4/SCHEME");
		request.get("driver-class").set("com.ibm.as400.access.AS400JDBCDriver");
		request.get("driver-name").set("jt400.jar");
		request.get("user-name").set("username");
		request.get("password").set("password");
		request.get("pool-name").set("pool_NewDatasource");
		ModelControllerClient client = ModelControllerClient.Factory.create(
				InetAddress.getByName("127.0.0.1"), 9999);
		client.execute(new OperationBuilder(request).build());
	}
```
If you want to check if the datasource already exists, consider following snippet:
``` java 
	public boolean checkIfDatasourceExists() throws Exception {
		ModelNode request = new ModelNode();
		request.get(ClientConstants.OP).set("read-resource");
		request.get("recursive").set(false);
		request.get(ClientConstants.OP_ADDR).add("subsystem", "datasources");
		ModelControllerClient client = ModelControllerClient.Factory.create(
				InetAddress.getByName("127.0.0.1"), 9999);
		ModelNode responce = client.execute(new OperationBuilder(request).build());
		ModelNode datasources = responce.get(ClientConstants.RESULT).get("data-source");
		if (datasources.isDefined()) {
			for (ModelNode dataSource : datasources.asList()) {
				String dataSourceName = dataSource.asProperty().getName();
				if (dataSourceName.equals("java:jboss/datasources/NewDatasource")) {
					return true;
				}
			}
		}
		return false;
	}
```
The examples have the same effect as trying to add datasource from Jboss Administration console.
