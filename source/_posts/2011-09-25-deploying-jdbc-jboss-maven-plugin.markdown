---
layout: post
title: Deploying JDBC driver in JBoss Maven plugin
categories:
- Java
- JBoss
tags:
- JBoss
- Maven
status: publish
type: post
published: true
comments: true

meta:
  _edit_last: '2'
  _wp_old_slug: deploying-jdbc-driver-in-jboss-maven-plugin
---
With release of jboss-as maven plugin version 7.0.1.Final the new goal deploy-artifact is added. It allows to deploy additional artifacts to JBoss server, like JDBC DB driver JAR.
After trying do deploy JDBC driver with jboss-as maven plugin using following code in pom.xml I received the <code>NullPointerException</code> in Maven build.
Here is part of maven pom.xml used to deploy the driver to jboss:
``` xml				
<plugin>
	<groupId>org.jboss.as.plugins</groupId>
	<artifactId>jboss-as-maven-plugin</artifactId>
	<version>7.0.1.Final</version>
	<executions>
		<execution>
			<id>deploy-driver-AS400</id>
			<phase>package</phase>
			<goals>
				<goal>deploy-artifact</goal>
			</goals>
			<configuration>
				<hostname>${jboss-hostname}</hostname>
				<groupId>com.ibm</groupId>
				<artifactId>as400</artifactId>
				<fileName>jt400.jar</fileName>
				<name>jt400.jar</name>
			</configuration>
		</execution>
	</executions>
</plugin>
```					
In the plugin's manual at <a href="http://docs.jboss.org/jbossas/7/plugins/maven/latest/examples/deployment-example.html">http://docs.jboss.org/jbossas/7/plugins/maven/latest/examples/deployment-example.html</a> it said
<blockquote>The artifact must be already listed as a dependency in the projects pom.xml</blockquote>
but it do not tells in which scope the driver's JAR have to be: it have to be in <strong>compile</strong> or <strong>runtime</strong> scope in order to work. I had it in provided scope. After changing the scope of dependency it works fine.
<!--more-->
Here is full stacktrace of the error I received:
```
[ERROR] Failed to execute goal org.jboss.as.plugins:jboss-as-maven-plugin:7.0.1.Final:deploy-artifact (deploy-driver-AS400) on project project-ear: Execution deploy-driver-AS400 of goal org.jboss.as.plugins:jboss-as-maven-plugin:7.0.1.Final:deploy-artifact failed. NullPointerException -> [Help 1]
org.apache.maven.lifecycle.LifecycleExecutionException: Failed to execute goal org.jboss.as.plugins:jboss-as-maven-plugin:7.0.1.Final:deploy-artifact (deploy-driver-AS400) on project project-ear: Execution deploy-driver-AS400 of goal org.jboss.as.plugins:jboss-as-maven-plugin:7.0.1.Final:deploy-artifact failed.
at org.apache.maven.lifecycle.internal.MojoExecutor.execute(MojoExecutor.java:225)
at org.apache.maven.lifecycle.internal.MojoExecutor.execute(MojoExecutor.java:153)
at org.apache.maven.lifecycle.internal.MojoExecutor.execute(MojoExecutor.java:145)
at org.apache.maven.lifecycle.internal.LifecycleModuleBuilder.buildProject(LifecycleModuleBuilder.java:84)
at org.apache.maven.lifecycle.internal.LifecycleModuleBuilder.buildProject(LifecycleModuleBuilder.java:59)
at org.apache.maven.lifecycle.internal.LifecycleStarter.singleThreadedBuild(LifecycleStarter.java:183)
at org.apache.maven.lifecycle.internal.LifecycleStarter.execute(LifecycleStarter.java:161)
at org.apache.maven.DefaultMaven.doExecute(DefaultMaven.java:319)
at org.apache.maven.DefaultMaven.execute(DefaultMaven.java:156)
at org.apache.maven.cli.MavenCli.execute(MavenCli.java:534)
at org.apache.maven.cli.MavenCli.doMain(MavenCli.java:196)
at org.apache.maven.cli.MavenCli.main(MavenCli.java:141)
at sun.reflect.NativeMethodAccessorImpl.invoke0(Native Method)
at sun.reflect.NativeMethodAccessorImpl.invoke(NativeMethodAccessorImpl.java:39)
at sun.reflect.DelegatingMethodAccessorImpl.invoke(DelegatingMethodAccessorImpl.java:25)
at java.lang.reflect.Method.invoke(Method.java:597)
at org.codehaus.plexus.classworlds.launcher.Launcher.launchEnhanced(Launcher.java:290)
at org.codehaus.plexus.classworlds.launcher.Launcher.launch(Launcher.java:230)
at org.codehaus.plexus.classworlds.launcher.Launcher.mainWithExitCode(Launcher.java:409)
at org.codehaus.plexus.classworlds.launcher.Launcher.main(Launcher.java:352)
Caused by: org.apache.maven.plugin.PluginExecutionException: Execution deploy-driver-AS400 of goal org.jboss.as.plugins:jboss-as-maven-plugin:7.0.1.Final:deploy-artifact failed.
at org.apache.maven.plugin.DefaultBuildPluginManager.executeMojo(DefaultBuildPluginManager.java:116)
at org.apache.maven.lifecycle.internal.MojoExecutor.execute(MojoExecutor.java:209)
... 19 more
Caused by: java.lang.NullPointerException
at org.jboss.as.plugin.deployment.AbstractDeployment.execute(AbstractDeployment.java:191)
at org.apache.maven.plugin.DefaultBuildPluginManager.executeMojo(DefaultBuildPluginManager.java:107)
... 20 more
```
