# Spring & Hibernate For Beginners

## Table of Contents
<<<<<<< HEAD
[Why Spring?](#user-content-why-spring)  <br />
[Overview of Spring](#overview-of-spring)  <br />
[Setting Up Spring](#setting-up-spring)  <br />
[Installing Tomcat](#installing-tomcat)  <br />
[Spring Inversion Of Control](#spring-inversion-of-control)  <br />
=======
[Why Spring?] (#why-spring)
[Overview of Spring](#overview-of-spring)
[Setting Up Spring](#setting-up-spring)
[Installing Tomcat](#installing-tomcat)
[Spring Inversion Of Control](#spring-inversion-of-control)
>>>>>>> 3b22de3dfd77ab9b17ada22b4c7f974d0e5f41d5


## Why Spring?
First of all, why spring?
Spring is a framework to build Java apps, much simpler than J2EE.

J2EE has:
- Client-side presentation.
- Server-side presentation.
- Server-side business logic.
- Database.

EJBs v1 and v2 were extremely complex.
They also were poor in performance.

So people resorted to developing WITHOUT EJB.
That's when Spring came out.
Along the way, Oracle fixed the stuff in J2EE and created Java EE.

Which should you learn, JEE or Spring?
Both, because you'll be more flexible in the market.
Also, a lot of skills are interchangeable between them.

So, let's get the show on the road.

## Overview of Spring

What are the goals of Spring?
- Lightweight dev with Java's very normal objects.
- Dependency injection to promote loose coupling.
- Aspect-Oriented-Programming (AOP).
- Minimize boilerplate code (Boilerplace is code that's repeated and common between projects).

### Contents of Spring Framework
1. Core Container. 
    - this is the heart of Spring.
    - it has the factory for creating beans.
    - it manages bean dependencies.
    - it makes beans available.
2. AOP section.
    - allow us to create app-wide services like Logging, Security, Transactions, etc.
3. Data Access Layer.
    - for communicating with a database (relational or NoSql).
    - it has some JDBC Helper classes that get the work done a lot faster.
    - ORM: object-to-relational-mapping, i.e. it integrates between Spring and Hibernate.
    - JMS: Java Message Service, allows us to send messages to a queue in an async way, it provides helper classes to help us deal with JMS.
    - Support for transactions.
4. Web layer.
    - the home of Spring MVC.
    - we have full MVC layout, classes, controllers, etc.
    - we can interface with other web tech too.
5. Instrumentation.
    - Spring has a lot of complex tech behind the scenes, we can use that here.
    - create java agents to remotely monitor the app using JMX (Java Management Extension).
6. Test layer.
    - support for TDD.
    - mock objects.
    - unit testing.
    - integration testing.

### Spring Projects
These are additional Spring modules built on top of the framework, they are like plugins or add-ons.
Some of them:
- Spring Boot. (This is a very popular one)
- Spring Cloud.
- Spring Batch.
- Spring for Android and Spring Web Flow.
- Spring Web Services.
etc.

Check the main website of Spring:
www.spring.io

You can check all the projects there.

## Setting Up Spring
Requirements for Spring:
1. JDK.
2. Java App Server.
    Examples: JBoss, WebLogic, Glassfish, etc.
    We'll use: Tomcat.
3. Java IDE.
    We'll use Eclipse.

Steps:
1. Install Tomcat.
2. Install Eclipse.
3. COnnect Eclipse to Tomcat.
4. Download Spring JAR files.

### Installing Tomcat
1. Download the binary distribution 64-bit Windows Service Installer: http://tomcat.apache.org/
2. Start the .msi file that was downloaded.
    - at Choose Components, choose "Full".
    - set a username and password.
    - choose path for JRE directory or JDK directory.
    - next, next, next.
3. Start tomcat.
4. Verify the installation by visiting localhost:8080, should bring up a fancy page that tells you that you succeeded in installation.

You can administer tomcat using Services in the windows control panel.
In the list, we'll see Apache Tomcat in the services, running.
Right-click and stop it, because we'll start it again using Eclipse.

### Installing Eclipse
1. Go to www.eclipse.org, choose Download, Download Packages, choose Eclipse For Java EE Developers, or Eclipse for Enterprise Java Developers, then click Download, and wait.
2. Extract the zip file anywhere, I put it at C:\eclipse
3. Inside it, you can actually start eclipse.
4. Select anywhere as your workspace, just a folder where all the source colde will be.
5. Check that when it opens, it's called Eclipse Java EE IDE for Web Developers, now you succeeded in installation.

### Connectiong Tomcat to Eclipse
In eclipse:
1. close the welcome tab.
2. go to bottom section, choose Servers tab.
3. click on the link to add a server.
4. in the window that opens, expand the folder Apache, choose Tomcat server with the version you have, then 'next'.
5. Choose the tomcat installation directory as the place where you installed tomcat (obviously xD), default is C:\Program Files\Apache Software Foundation\Tomcat <version number>
Then click 'next'
6. Click 'finish'.

### Download Spring JAR files
The main steps are:
1. Create an eclipse project.
2. Download Spring JAR files.
3. Add those JAR files to our eclipse project.

Why not use Maven?
- If you're a Maven expert, use it.
- If not, follow through with the steps here.
(BTW, at the end of the course, we'll go through Maven)

Steps:
1. open the Window menu.
2. Perspective -> Open Perspective -> Java.
3. Open the File menu.
4. New -> Java Project.
5. Name it 'spring-demo-one' or anything you want.
6. 'finish'
7. Now we'll download Spring JAR files:
https://repo.spring.io/release/org/springframework/spring/

Or use the link:
www.luv2code.com/downloadSpring
Which will redirect you to the link above.

8. Scroll to the bottom and choose the latest release, then choose RELEASE-dist.zip.
9. Wait for download.
10. Unzip the file anywhere you want.
11. Go into the folder/libs, there's a lot of JAR files.
12. Select and copy ALL of these JAR files (Ctrl + A then Ctrl + C)
13. Move to eclipse, to the project you made, right-click -> New -> Folder, create a folder caled 'lib'.
14. right-click on lib -> Paste.
15. right-click on the project -> Properties.
16. Choose Java Build Path.
17. Choose the Libraries tab.
18. Choose Classpath, click Add JARs.
19. In the window that opened, go into lib, select all jars, click OK, then OK.
20. You'll see a new icon in the directory tree called 'Referenced Libraries', that references all the JAR files we downloaded.

Now, you're done setting up Spring.

## Spring Inversion Of Control
What is Inversion of Control?
It's externalizing the construction and management of object.
i.e. your app will outsource creation and management of objects, which will be handled by object factories.

If we have an app that communicates with a BaseballCoach that gives you a workout.
But the coach should be configurable, like TennisCoach, SoccerCoach, etc.

Rough implementation:
``` Java
public class BaseballCoach {
	public String getDailyWorkout() {
		return "Insanity";
	}
}


public class MyApp {

	public static void main(String[] args) {
		// TODO Auto-generated method stub
		BaseballCoach theCoach = new BaseballCoach();
		System.out.println(theCoach.getDailyWorkout());
	}

}

```
Instead of that, we could program to an interface to make all coaches work with MyApp:

Check this out:
``` Java
public interface Coach {
	public String getDailyWorkout();
}

public class BaseballCoach implements Coach {

    @Override
	public String getDailyWorkout() {
		return "Insanity";
	}
}

// We can add another type of coach
public class TrackCoach implements Coach {

	@Override
	public String getDailyWorkout() {
		// TODO Auto-generated method stub
		return "Run 10 miles per hour";
	}

}


public class MyApp {

	public static void main(String[] args) {
		// TODO Auto-generated method stub
		Coach theCoach = new BaseballCoach();

        // If you change it to:
        // Coach theCoach = new TrackCoach();
        // It will work correctly :)

		System.out.println(theCoach.getDailyWorkout());
	}

}
```
That's better.
But the app isn't configurable, we're still changing CODE between TrackCoach and BaseballCoach.

It would be convenient to read the implementation name from a config file and act upon it.
Maybe we can do that using Reflection, But we'll focus on how Spring addresses this problem.

### Spring Container a.k.a. *ApplicationContext*
It:
- Creates and manages objects (inversion of control).
- Injects object's dependencies (dependency injection).

How to configure Spring Container:
- XML config file (legacy, but a lot of legacy apps use it).
- Through annotations.
- Through source code.

We'll use that, we'll talk to the Spring Container so that it gives us the correct Coach. 

#### What the hell is a Spring Bean?
A "Spring Bean" is simply a Java object, created from normal Java classes, they have dependencies, etc. They are just java object instances.
When a Java Object is created by the Spring Container, it's called a Spring Bean.
It's just an object that's being managed by the Container.

More info:
https://docs.spring.io/spring/docs/current/spring-framework-reference/core.html#beans-introduction

#### Spring Development Process
1. Configure your Spring Beans.
2. Create a Spring Container.
    Some specialized implementations:
    - ClassPathXmlApplicationContext.
    - AnnotationConfigApplicationContext.
    - GenericWebApplicationContext.
    - and others. (VanossGaming reference xD)
3. Retrieve Beans from the Container.

#### Back to our app
Earlier we downloaded the course zip file, which has a lot of files in it, called "spring-and-hibernate-source-code".

Inside it -> spring-core -> spring-demo-one -> applicationContext.xml

Copy that, paste it into the src directory of your project.

Open it, it's an XML file.
Now, we'll define our beans, first step in our spring dev process:
``` XML
<bean id="myCoach"
    class="com.luv2code.springdemo.TrackCoach">
</bean>
```

Now, we'll use a Java class, we'll create a new class called HelloSpringApp.
In it, we'll do the next spring dev steps:
2. Load the spring config file.
3. Retrieve bean from spring container.
``` Java
import org.springframework.context.support.ClassPathXmlApplicationContext;

public class HelloSpringApp {

	public static void main(String[] args) {
		
		// Load the config file
		ClassPathXmlApplicationContext context = 
				new ClassPathXmlApplicationContext("applicationContext.xml");

		// Retrieve your bean
		Coach theCoach = context.getBean("myCoach", Coach.class);
		
		// do stuff with the bean
		System.out.println(theCoach.getDailyWorkout());
	}

}
```

Run that, it'll work.  <br />
NOTE: If you see red log messages, don't worry that's normal :) you can make them appear if you want, we'll discuss that later.

