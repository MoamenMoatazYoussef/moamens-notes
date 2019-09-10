# Spring & Hibernate For Beginners

## Table of Contents
- [Spring & Hibernate For Beginners](#spring---hibernate-for-beginners)
  * [Table of Contents](#table-of-contents)
  * [Notes before you read](#notes-before-you-read)
  * [Why Spring?](#why-spring-)
  * [Overview of Spring](#overview-of-spring)
    + [Contents of Spring Framework](#contents-of-spring-framework)
    + [Spring Projects](#spring-projects)
  * [Setting Up Spring](#setting-up-spring)
    + [Installing Tomcat](#installing-tomcat)
    + [Installing Eclipse](#installing-eclipse)
    + [Connectiong Tomcat to Eclipse](#connectiong-tomcat-to-eclipse)
    + [Download Spring JAR files](#download-spring-jar-files)
  * [Spring Inversion Of Control](#spring-inversion-of-control)
    + [Spring Container](#spring-container)
      - [What the hell is a Spring Bean?](#what-the-hell-is-a-spring-bean-)
      - [Spring Development Process](#spring-development-process)
      - [Back to our app](#back-to-our-app)
  * [Spring Dependency Injection](#spring-dependency-injection)
    + [Constructor Injection](#constructor-injection)
      - [Step 1: Define the dependency's interface and class](#step-1--define-the-dependency-s-interface-and-class)
      - [Step 2: Create a constructor in our class for injections](#step-2--create-a-constructor-in-our-class-for-injections)
      - [Step 3: Configure the depencency injection in Spring config file](#step-3--configure-the-depencency-injection-in-spring-config-file)
    + [Setter Injection](#setter-injection)
      - [Step 1: Create setter methods that'll be used for injections](#step-1--create-setter-methods-that-ll-be-used-for-injections)
      - [Step 2: Configure the dependency injection in the config file](#step-2--configure-the-dependency-injection-in-the-config-file)
    + [Injecting Literal Values in Spring Objects](#injecting-literal-values-in-spring-objects)
      - [Step 1: Create setter methods in our class for injections](#step-1--create-setter-methods-in-our-class-for-injections)
      - [Step 2: Configure the config file](#step-2--configure-the-config-file)
    + [Injecting Values from a Properties File](#injecting-values-from-a-properties-file)
      - [Step 1: Create the Properties file](#step-1--create-the-properties-file)
      - [Step 2: Load the properties file into the Spring config file](#step-2--load-the-properties-file-into-the-spring-config-file)
      - [Step 3: Reference the values from the Properties file](#step-3--reference-the-values-from-the-properties-file)
  * [Spring Bean Scopes and Lifecycle](#spring-bean-scopes-and-lifecycle)
    + [Spring Bean Scope](#spring-bean-scope)
    + [Bean Lifecycle Methods](#bean-lifecycle-methods)
      - [Step 1: Define the init and destroy methods](#step-1--define-the-init-and-destroy-methods)
      - [Step 2: Configure the method names in the config file](#step-2--configure-the-method-names-in-the-config-file)
  * [Spring Configuration With Java Annotations](#spring-configuration-with-java-annotations)
    + [Component Scanning](#component-scanning)
      - [This is getting messy](#this-is-getting-messy)
      - [Step 1: Enable component scanning in Spring config file](#step-1--enable-component-scanning-in-spring-config-file)
      - [Step 2: Add @Component annotation for java classes](#step-2--add--component-annotation-for-java-classes)
      - [Step 3: Retrieve the bean from the container](#step-3--retrieve-the-bean-from-the-container)
    + [Using Default Component Names](#using-default-component-names)
      - [What happens if](#what-happens-if)
  * [Dependency Injection With Annotations](#dependency-injection-with-annotations)
    + [Construction Injection with Annotations](#construction-injection-with-annotations)
      - [Step 1: Define the dependency interface or class](#step-1--define-the-dependency-interface-or-class)
      - [Step 2 and 3: Creata a constructor in your class for injections, then configure the dependency injection with @Autowired annotation.](#step-2-and-3--creata-a-constructor-in-your-class-for-injections--then-configure-the-dependency-injection-with--autowired-annotation)
    + [Setter Injection with Annotations](#setter-injection-with-annotations)
    + [Method Injection with Annotations](#method-injection-with-annotations)
    + [Field Injection with Annotations](#field-injection-with-annotations)
    + [Which type of injection should I use?](#which-type-of-injection-should-i-use-)
    + [Qualifiers for Dependency Injection](#qualifiers-for-dependency-injection)
  * [Bean Scope and Lifecycle with Annotations](#bean-scope-and-lifecycle-with-annotations)
    + [Bean Scope with Annotations](#bean-scope-with-annotations)
    + [Bean Lifecycle with Annotations](#bean-lifecycle-with-annotations)
  * [Spring Configuration With Java Code (no xml)](#spring-configuration-with-java-code--no-xml-)
    + [Java Config Class with Component Scaning](#java-config-class-with-component-scaning)
    + [Java Config Class with Manual Bean Configuration](#java-config-class-with-manual-bean-configuration)
      - [Java Config Class with Manual Bean Configuration - Example](#java-config-class-with-manual-bean-configuration---example)
    + [Java Config Class with injecting Values by a Properties File](#java-config-class-with-injecting-values-by-a-properties-file)
      - [Step 1: Create a properties file](#step-1--create-a-properties-file)
      - [Step 2: Load it in Java Config Class](#step-2--load-it-in-java-config-class)
      - [Step 3: Reference its values](#step-3--reference-its-values)
  * [Spring MVC - Building Spring Web Apps](#spring-mvc---building-spring-web-apps)
    + [What is Spring MVC?](#what-is-spring-mvc-)
    + [Components of Spring MVC](#components-of-spring-mvc)
    + [Setting Up Spring MVC](#setting-up-spring-mvc)
    + [Spring MVC Configuration](#spring-mvc-configuration)
    + [Spring MVC: Creating Controllers and Views](#spring-mvc--creating-controllers-and-views)
      - [Step 1: Create controller class](#step-1--create-controller-class)
      - [Step 2: Define controller method](#step-2--define-controller-method)
      - [Step 3: Add request mapping to the controller method](#step-3--add-request-mapping-to-the-controller-method)
      - [Step 4: Return the view name](#step-4--return-the-view-name)
      - [Step 5: Develop view page](#step-5--develop-view-page)

<small><i><a href='http://ecotrust-canada.github.io/markdown-toc/'>Table of contents generated with markdown-toc</a></i></small>


## Notes before you read
- You'll see me writing "sysout" a lot, sysout means System.out.println.
-----

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

### Spring Container
It:
- Creates and manages objects (inversion of control).
- Injects object's dependencies (dependency injection).

How to configure Spring Container:
- xml config file (legacy, but a lot of legacy apps use it).
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

Open it, it's an xml file.
Now, we'll define our beans, first step in our spring dev process:
``` xml
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
        // Why do we specify the interface here?
        // When we pass the interface, Spring will cast the object returned for us.
        // But not normal casting, 
        // there is a method getBean(String), without passing the interface.
        // but the one WITH the interface throws a BeanNotOfRequiredTypeException
        // if the bean is not of the required type.
        // Which is useful
        // Read more here:
        // https://docs.spring.io/spring-framework/docs/current/javadoc-api/org/springframework/beans/factory/BeanFactory.html#getBean-java.lang.String-java.lang.Class-
		
		// do stuff with the bean
		System.out.println(theCoach.getDailyWorkout());

		context.close();
	}

}
```

Now that's a pretty code <3  <br />
Run that, it'll work.  <br />

Try to change the Bean class to BaseballCoach in applicationContext.xml, and run your app again, see the results.

Now our app is:
- configurable.
- can plug with any type of coach.

Yeah we're awesome B|  <br />


***Note:***
If you see red log messages, don't worry that's normal :) you can make them appear if you want:
1. Create a bean to configure the parent logger and console handler

This class will set the parent logger level for the application context. It will also set the logging level for console handler. It sets the logger level to FINE. For more detailed logging info, you can set the logging level to level to FINEST.  You can read more about the logging levels at http://www.vogella.com/tutorials/Logging/article.html

This class also has an init method to handle the actual configuration. The init method is executed after the bean has been created and dependencies injected.

File: MyLoggerConfig.java
``` Java
package com.luv2code.springdemo;
 
import java.util.logging.ConsoleHandler;
import java.util.logging.Level;
import java.util.logging.Logger;
import java.util.logging.SimpleFormatter;
 
import org.springframework.context.annotation.AnnotationConfigApplicationContext;
 
public class MyLoggerConfig {
 
	private String rootLoggerLevel;
	private String printedLoggerLevel;
	
	public void setRootLoggerLevel(String rootLoggerLevel) {
		this.rootLoggerLevel = rootLoggerLevel;
	}
 
	public void setPrintedLoggerLevel(String printedLoggerLevel) {
		this.printedLoggerLevel = printedLoggerLevel;
	}
 
	public void initLogger() {
 
		// parse levels
		Level rootLevel = Level.parse(rootLoggerLevel);
		Level printedLevel = Level.parse(printedLoggerLevel);
		
		// get logger for app context
		Logger applicationContextLogger = Logger.getLogger(AnnotationConfigApplicationContext.class.getName());
 
		// get parent logger
		Logger loggerParent = applicationContextLogger.getParent();
 
		// set root logging level
		loggerParent.setLevel(rootLevel);
		
		// set up console handler
		ConsoleHandler consoleHandler = new ConsoleHandler();
		consoleHandler.setLevel(printedLevel);
		consoleHandler.setFormatter(new SimpleFormatter());
		
		// add handler to the logger
		loggerParent.addHandler(consoleHandler);
	}
	
}
---
```

2. Configure the bean in the Spring xml config file

In your xml config file, add the following bean entry. Make sure to list this as the first bean so that it is initialized first. Since the bean is initialized first, then you will get all of the logging traffic. If you move it later in the config file after the other beans, then you will miss out on some of the initial logging messages.

File: applicationContext.xml (snippet)
``` xml
<!-- 
	Add a logger config to see logging messages.		
	- For more detailed logs, set values to "FINEST"
	- For info on logging levels, see: http://www.vogella.com/tutorials/Logging/article.html
 -->
    <bean id="myLoggerConfig" class="com.luv2code.springdemo.MyLoggerConfig" init-method="initLogger">
    	<property name="rootLoggerLevel" value="FINE" />
    	<property name="printedLoggerLevel" value="FINE"/>
    </bean>
```
Source code is available here.
https://gist.github.com/darbyluv2code/cfb16c2fd1825a947d8faca3724b47a9

Once you make these updates, then you will be able to see additional logging data. :-)

## Spring Dependency Injection
So what exactly is that?
If I'm going to buy a car, and that car will be made in the factory on demand, in the factory there's all the different parts of the car.
The mechanics assemble the car, they inject all of the dependencies of the car, like the engine, tires, seats, etc. then they deliver the car for you.
You don't have to build the car yourself.

Dependency injection is the same, Spring has an object factory,
When we retrieve the coach object, it may have other dependencies, other objects needed for the coach object's construction.
The Spring framework gets those objects and constructs the coach object.
What you get is a ready-to-use object.

In our example, our coach provides daily workouts.
Now, the coach will provide Fortunes, and to do that, they'll need the help of a HELPER called FortuneService.

The helper is called "dependency".

There are a lot of injection types in Spring, two of the most used are:
- Constructor Injection.
- Setter Injection.

### Constructor Injection 
Steps:
1. Define the dependency's interface and class.
2. Create a constructor in our class for injections.
3. Configure the depencency injection in Spring config file.

We'll see how that is done in our example.

#### Step 1: Define the dependency's interface and class
First, we need to define the dependency interface:
``` Java 
public interface FortuneService {
	public String getFortune();
}
```

Then we create the dependency class:
``` Java
public class HappyFortune implements FortuneService {

	@Override
	public String getFortune() {
		// TODO Auto-generated method stub
		return "You don't know the power of the dark side";
	}

}
```

Now, modify the coach interface to make them able to Give fortune:
``` Java
public interface Coach {
	public String getDailyWorkout();
	public String getDailyFortune();
}
```

That will cause errors in BaseballCoach and TrackCoach because the new method isn't implemented in them, so we go and add them, let them return null for now.

#### Step 2: Create a constructor in our class for injections
First, in BaseballCoach, we'll define a private FortuneService reference, 
then add a constructor that accepts that reference,
finally, we'll use that in our getDailyFortune method:
``` Java
public class BaseballCoach implements Coach {
	
	public FortuneService fortuneService;
	
	public BaseballCoach(FortuneService theFortuneService) {
		fortuneService = theFortuneService;
	}
	
	@Override
	public String getDailyWorkout() {
		return "Insanity";
	}

	@Override
	public String getDailyFortune() {
		return fortuneService.getFortune();
	}
}
```
Nice. <br />
Do the same for TrackCoach and add a message to the fortune or whatever. <br />

#### Step 3: Configure the depencency injection in Spring config file

Add the bean to applicationContext.xml:
``` xml
<bean id="myFortuneService"
    class="com.luv2code.springdemo.HappyFortuneService">
</bean>
```

Now, in the coach bean, we'll do the actual injection:
``` xml
    <bean id="myCoach"
    	class="com.luv2code.springdemo.TrackCoach">
    	<constructor-arg ref="myFortuneService"></constructor-arg>
    </bean>
```

Now, go to HellpSpringApp class, just print out theCoach.getDailyFortune(), see the result :)

### Setter Injection 
Spring injects dependencies by calling setters on your class.

Steps:
1. Create setter methods that'll be used for injections.
2. Configure the dependency injection in the config file.

#### Step 1: Create setter methods that'll be used for injections
First, we'll create a new class: CricketCoach.
Inside it we'll add the setter.
``` Java
public class CricketCoach implements Coach {

	private FortuneService fortuneService;
	
	// We need the no arg constructor since Spring will use it
	// to create the instance
	public CricketCoach() {
		// That is just for debugging, no other purpose
		System.out.println("Inside the constructor of CricketCoach");
	}
	
	// Step 1 here, just a normal setter for our dependency
	public void setFortuneService(FortuneService fortuneService) {
		this.fortuneService = fortuneService;
	}

	@Override
	public String getDailyWorkout() {
		return "Practice fast bowling";
	}

	@Override
	public String getDailyFortune() {
		return "Darth...Vader, " + fortuneService.getFortune();
	}
}
```

#### Step 2: Configure the dependency injection in the config file
In the config file, add the CricketCoach bean and add a <property > tag like this:
``` xml
    <bean id="myCricketCoach"
    	class="com.luv2code.springdemo.CricketCoach">
    	
    	<!-- Setting up out setter injection -->
    	<property name="fortuneService" ref="myFortuneService"></property>
    	<!-- That will call the setter, setFortuneService -->
    	
    </bean>
```

Finally, make the MyApp class like this:
``` Java
import org.springframework.context.support.ClassPathXmlApplicationContext;

public class MyApp {
	public static void main(String[] args) {
		ClassPathXmlApplicationContext context = 
				new ClassPathXmlApplicationContext("applicationContext.xml");

		Coach theCoach = context.getBean("myCricketCoach", Coach.class);
		
		System.out.println(theCoach.getDailyWorkout());
		System.out.println(theCoach.getDailyFortune());
		
		context.close();
	}
}
```

Run it, it'll be as awesome as me B| <br />
Nice.

### Injecting Literal Values in Spring Objects 
What if we want to inject values, like Strings, etc.?

Steps:
1. Create setter methods in our class for injections.
2. Configure the config file.

#### Step 1: Create setter methods in our class for injections
We'll do that in CricketCoach, 
first we'll add some private fields like emailAddress, team.

``` Java

public class CricketCoach implements Coach {

	private FortuneService fortuneService;
	private String emailAddress;
	private String team;
	
	public CricketCoach() {
		System.out.println("Inside the constructor of CricketCoach");
	}

	public String getEmailAddress() {
		return emailAddress;
	}
	
	public void setEmailAddress(String emailAddress) {
		System.out.println("Setting email address to: " + emailAddress);
		this.emailAddress = emailAddress;
	}

	public String getTeam() {
		return team;
	}

	public void setTeam(String team) {
		System.out.println("Setting team to: " + team);
		this.team = team;
	}

	
	public void setFortuneService(FortuneService fortuneService) {
		this.fortuneService = fortuneService;
	}

	@Override
	public String getDailyWorkout() {
		return "Practice fast bowling";
	}

	@Override
	public String getDailyFortune() {
		return "Darth...Vader, " + fortuneService.getFortune();
	}

}
```
#### Step 2: Configure the config file
Now, in the config file, do that:
``` xml
	<bean id="myCricketCoach"
    	class="com.luv2code.springdemo.CricketCoach">
    	
    	<!-- Setting up out setter injection -->
    	<property name="fortuneService" ref="myFortuneService"></property>
    	<!-- That will call the setter, setFortuneService -->
    	
    	<!-- Values that will be injected -->
    	<property name="emailAddress" value="awesome.team@gmail.com"></property>
    	<property name="team" value="New York Giants"></property>
    </bean>
```
And update the MyApp class:
``` Java
public class MyApp {
	public static void main(String[] args) {
		ClassPathXmlApplicationContext context = 
				new ClassPathXmlApplicationContext("applicationContext.xml");

		// Notice that we had to change the type to CricketCoach
		// because a reference to Coach doesn't look at methods that were
		// not declared inside the interface e.g. getTeam and getEmailAddress
		// which are the methods we'll use to test our code.
		// Also, the type passed in getBean has changed too. 
		CricketCoach theCoach = context.getBean("myCricketCoach", CricketCoach.class);
		
		System.out.println(theCoach.getDailyWorkout());
		System.out.println(theCoach.getDailyFortune());
		
		System.out.println(theCoach.getEmailAddress());
		System.out.println(theCoach.getTeam());
		
		context.close();
	}
}
```

Nice. :) <br />

### Injecting Values from a Properties File 
Finally, we'll look at how we can inject values but without hardcoding them into the config file, we'll read them from a Properties file.

Steps:
1. Create the Properties file.
2. Load the properties file into the Spring config file.
3. Reference the values from the Properties file.

#### Step 1: Create the Properties file
Create a new file of type File, call it sport.properties.
Write this into it:
``` txt
foo.email=really.awesome.team@gmail.com
foo.team=Texas Bulls
```
Easiest thing of my life.

#### Step 2: Load the properties file into the Spring config file
Now, in our config file, just before our beans, add this:
``` xml
<context:property-placeholder location="classpath:sport.properties"/>
```

#### Step 3: Reference the values from the Properties file
In the config file, replace the things passed to the value attribute with these:
Notice what's being passed to the value attribute in the <property> tags:
``` xml
    <bean id="myCricketCoach"
    	class="com.luv2code.springdemo.CricketCoach">
    	
    	<!-- Setting up out setter injection -->
    	
    	<property name="fortuneService" ref="myFortuneService"></property>
    	<!-- That will call the setter, setFortuneService -->
    	
    	<!-- Values that will be injected -->
    	
    	<property name="emailAddress" value="${foo.email}"></property>
    	<property name="team" value="${foo.team}"></property>
    </bean>
```

Nice work. :) <br />

## Spring Bean Scopes and Lifecycle
### Spring Bean Scope
The Bean Scope is the lifecycle of a bean:
- How long it will live.
- How many instances of it are created.
- How is it shared.

Default scope is: Singletong

What is Singleton?
- Creating ONE INSTANCE of the bean.
- Cached in memory.
- All requests for the bean returns a reference to that one instance.

This is good for STATELESS BEANS i.e. we don't want to keep any state.

We can specify the scope by adding a scope attribute in the bean like: scope="singleton".

Othe scopes:
- prototype: creates an instance each time the container requests a bean. (Good for STATEFUL BEANS)
- request, session, and global-session: scoped to an HTTP web request.

In our example we'll try the scopes out:
- Copy the config file and paste it in the same place, naming it beanScope-applicationContext.xml
- In the new file, remove the properties file line and the myCricketCoach.
- Create another main app class called BeanScopeDemoApp.java

Steps to do in the new app:
- Load the new config file.
- retrieve bean from container. Actually we'll retrieve TWO BEANS from the container:
``` Java
public class BeanScopeDemoApp {

	public static void main(String[] args) {
		ClassPathXmlApplicationContext context 
			= new ClassPathXmlApplicationContext("beanScope-applicationContext.xml");
		
		Coach theCoach = context.getBean("myCoach", Coach.class);
		Coach alphaCoach = context.getBean("myCoach", Coach.class);

		boolean isSingleton = (theCoach == alphaCoach);
		System.out.println(isSingleton);
		
		context.close();
	}

}
``` 

Default scope is singleton, so theCoach and alphaCoach should be referencing the same bean, right?

Yes :)
Check the output of isSingleton.

Let's change the bean scope to Prototype and see the result:
``` xml
    <bean id="myCoach"
    	class="com.luv2code.springdemo.TrackCoach" scope="prototype">
    	<constructor-arg ref="myFortuneService"></constructor-arg>
    </bean>
```
The result will be false :o

### Bean Lifecycle Methods
- Beans are instantiated.
- Dependencies are injected.
- Internal Spring processing occuring in the bean factory.
- **Your custom init method**
- *Bean is ready to use*
- **Your custom destroy method**

As you can see, we can do a custom init method WHEN the bean is created, in it we can:
- Call custom business logic methods.
- Set up handles to resources like db, network, etc.

We can also do the same before the bean is destroyed.

Both methods can have any access modifier and any return type, but void is common because you usually are not able to capture the return value.
Both methods cannot accept any arguments.

How do we do this?
In the config file, in the bean, we can add two attributes:
- init-method
- destroy-method

Steps to do that:
- Define the init and destroy methods.
- Configure the method names in the config file.

#### Step 1: Define the init and destroy methods
We'll add the methods in our TrackCoach class:
``` Java
public class TrackCoach implements Coach {
	private FortuneService fortuneService;
//	private int id;
	
	public TrackCoach() {
		
	}
	
	public TrackCoach(FortuneService theFortuneService) {
		fortuneService = theFortuneService;
	}

	@Override
	public String getDailyWorkout() {
		return "Run 10 miles per hour";
	}

	@Override
	public String getDailyFortune() {
		return "Luke, " + fortuneService.getFortune();
	}
	
	// define init method
	public void doMyStartupStuff() {
		System.out.println("Initializing the bean - custom mode");
	}
	
	// define destroy method
	public void doMyDestroyStuff() {
		System.out.println("Destroying the bean - custom mode");
	}
}
```

#### Step 2: Configure the method names in the config file
Copy and paste the config file, name the new one "beanLifecycle-applicationContext.xml"

***Note:*** as you can see, we can have many config files in one project.

Inside it, remove the prototype scope from earlier (Now it's default, which is Singleton).
Then add the two attributes:
``` xml
    <bean id="myCoach"
    	class="com.luv2code.springdemo.TrackCoach" 
    	init-method="doMyStartupStuff"
    	destroy-method="doMyDestroyStuff">
    	<constructor-arg ref="myFortuneService"></constructor-arg>
    </bean>
```

Then make a new main class, copy the BeanScopeDemoApp and name the new class BeanLifecycleDemoApp:
``` Java
public class BeanLifecycleDemoApp {
	public static void main(String[] args) {
		ClassPathXmlApplicationContext context 
			= new ClassPathXmlApplicationContext("beanLifecycle-applicationContext.xml");
		
		Coach theCoach = context.getBean("myCoach", Coach.class);
		Coach alphaCoach = context.getBean("myCoach", Coach.class);

		boolean isSingleton = (theCoach == alphaCoach);
		System.out.println(isSingleton);
		
		context.close();
	}

}

```

Now run it, it will print the sysout statements in the init and destroy methods we defined :)

***Note:*** If you created theCoach and alphaCoach, and we removed the prototype scope, you'll notice that the output will print the sysout statements in our init and destroy methods once, this means that both reference the same bean i.e. Singleton scope ;)

Also, the destroy sysout statement is printed when we execute context.close()

***Note:*** 
- If the scope is prototype, spring does **not** call the destroy method.
- Because spring doesn't manage the complete lifecycle of a prototype bean.
- So, our code must clean up prototype-scoped object and release resources that they were holding.
- To get the Spring container to release resources held by prototype-scoped beans, try using a custom bean post-processor, which holds a reference to beans that need to be cleaned up, check this out:
https://docs.spring.io/spring/docs/current/spring-framework-reference/core.html#beans-factory-extension-bpp

## Spring Configuration With Java Annotations
See annotations from Java Core note, here:
https://bit.ly/2m8tKj4

Why use annotations with Spring configuration?
- xml can be very verbose (exxessively detailed and tiring) for large projects.
- Using annotations for configurations minimize the xml configuration.

### Component Scanning

When we add an annotation to a class:
- Spring will scan classes for annotation.
- When it finds an anootation for a class, it registers the bean instantiated from that class with those annotations.

Steps:
- Enable component scanning in Spring config file.
- Add @Component annotation for java classes.
- Retrieve the bean from the container.

#### This is getting messy
Since things are getting complex, we'll separate them a little bit.
First, we'll make a brand new project in Eclipse to separate things a little bit.
Call it spring-demo-annotations or something.
Copy the lib directory from spring-demo-one (i.e. the first project), to the new project.
Configure the build path just like before.
Add a package to it, call it "com.luv2code.springdemo".

#### Step 1: Enable component scanning in Spring config file
Copy the config file from the previous project "applicationContext.xml", to the new project.

***Note:*** It's important that the config files are directly in the src directory.

Remove all the bean entries and the properties file.
Close the previous project, right click on it and choose Close.

Now go to the config file in the new project (The one we pasted).
Add this line to enable component scanning:
``` xml
<context:component-scan base-package="com.luv2code.springdemo" />
```

#### Step 2: Add @Component annotation for java classes
First, we'll create the Coach interface just like before.
Then we'll create a TennisCoach class that implements it.
And we'll add the @Component annotation to it, giving it a "bean id" argument, which Spring will use to retrieve the bean after that:
``` Java
import org.springframework.stereotype.Component;

@Component("thatSillyCoach")
public class TennisCoach implements Coach {

	@Override
	public String getDailyWorkout() {
		return "You can do it, Rafael";
	}

}
```

#### Step 3: Retrieve the bean from the container
Create a new main class, call it "AnnotationDemoApp" or something.
First, read the config file.
Then, get the bean USING the "bean id" argument passed to the @Component annotation.
Then call some methods on the bean.
And close the context.
``` Java
import org.springframework.context.support.ClassPathXmlApplicationContext;

public class AnnotationDemoApp {

	public static void main(String[] args) {
		ClassPathXmlApplicationContext context = 
			new ClassPathXmlApplicationContext("applicationContext.xml"); 

		// Notice we used the bean id we passed in the component annotation		
		Coach theCoach = context.getBean("thatSillyCoach", Coach.class);
		
		System.out.println(theCoach.getDailyWorkout());
		
		context.close();
	}

}
```
Now run it, and it'll work :) <br />

*This is how we do it * <br />

### Using Default Component Names
We passed an explicit ID to the @Component annotation.
Instead of that, we can let Spring generate a Default Bean ID, which is the same as the class name but in Camel case i.e. first letter is Lowercase.

How to do that:
- Just remove the bean ID and the brackets.
``` Java
// Notice the difference here
@Component
public class TennisCoach implements Coach {

	@Override
	public String getDailyWorkout() {
		return "You can do it, Rafael";
	}

}
```

- Retrieve the bean using the default ID.
```Java
import org.springframework.context.support.ClassPathXmlApplicationContext;

public class AnnotationDemoApp {

	public static void main(String[] args) {
		ClassPathXmlApplicationContext context = 
			new ClassPathXmlApplicationContext("applicationContext.xml"); 
		
		// Notice the difference here
		Coach theCoach = context.getBean("tennisCoach", Coach.class);
		
		System.out.println(theCoach.getDailyWorkout());
		
		context.close();
	}

}
```

That's it :) <br />

***Note:*** As you can see, here we're outsourcing the bean creation i.e. Inversion of control

#### What happens if
- You pass a wrong ID to the context.getbean?
	- NoSuchBeanDefinitionException will be raised.

## Dependency Injection With Annotations
There's a thing in Spring called Autowiring, which is used for dependency injection with annotations.

What is AutoWiring?
- Spring automatically looks for a class that matches the given property, and matches them by type (Class or interface).
- If they match, Spring will inject that property into this class automatically.

For example, we want to inject FortuneService into Coach implementation:
- Spring scans @Components.
- Any of them implements FortuneService interface?
- Yes? inject it in.

Types of Autowiring:
- Constructor Injection.
- Setter Injection.
- Field Injection.
Just like normal injection.

Steps for Constructor Injection:
- Define the dependency interface/class.
- Creata a constructor in your class for injections.
- Configure the dependency injection with @Autowired annotation.

***Note:*** What if there are many FortuneService implementations?
Spring has a special annotation @Qualifier, which chooses which implementation to inject in which bean, we'll cover that later.

### Construction Injection with Annotations
#### Step 1: Define the dependency interface or class
Create a new interface FortuneService just like before.
``` Java
public interface FortuneService {
	public String getFortune();
}
```
Now create a class that'll implement that interface, HappyFortuneService also just like before.
BUT, add the @Component annotation.
``` Java
import org.springframework.stereotype.Component;

@Component
public class HappyFortuneService implements FortuneService {

	@Override
	public String getFortune() {
		return "Unpredictable fortune, my past";
	}
}
```

Really basic stuff for us now cuz we're awesome B|

Now, we'll add a method to the Coach interface, getDailyFortune(), juuust like before.
``` Java
public interface Coach {
	public String getDailyWorkout();
	public String getDailyFortune();
}
```

#### Step 2 and 3: Creata a constructor in your class for injections, then configure the dependency injection with @Autowired annotation.
Now the TennisCoach class will break since it doesn't fully implement Coach now, so we'll fix that, by:
- Adding a private FortuneService field to TennisCoach.
- Implementing getDailyFortune() method and use the new field in it.
- Create an constructor that takes a FortuneService as argument.
- Add the @Autowired annotation.
``` Java
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.stereotype.Component;

@Component
public class TennisCoach implements Coach {
	private FortuneService fortuneService;
	
	@Autowired
	public TennisCoach(FortuneService fs) {
		fortuneService = fs;
	}
	
	@Override
	public String getDailyWorkout() {
		return "You can do it, Rafael";
	}

	@Override
	public String getDailyFortune() {
		return fortuneService.getFortune();
	}

}
```
Spring will SCAN the components to see what implements the FortuneService interface.
It will find HappyFortuneService.
So it will inject that into TennisCoach.

Now, in your main app, just add 
System.out.println(theCoach.getDailyFortune());

And run it. It'll work, cuz we work B| <br />

***Note:***
We can remove the @Autowired annotation and it would still work, why?
As of Spring Framework 4.3, an @Autowired annotation on such a constructor is no longer necessary if the target bean only defines one constructor to begin with. However, if several constructors are available, at least one must be annotated to teach the container which one to use.

I personally prefer to use the @Autowired annotation because it makes the code more readable. But as mentioned, the @Autowired is not required for this scenario.

See link to the docs:
https://docs.spring.io/spring/docs/current/spring-framework-reference/core.html#beans-autowired-annotation

### Setter Injection with Annotations
Steps:
- Create setter methods for injections.
- Configure dependency injection with @Autowired annotation.

Go to TennisCoach class, comment out the constructor we added last time, with the annotation.
Add a no-arg constructor, put any sysout in it or something.
Add a setter for FortuneService field.
Then add the @Autowired annotation.
``` Java
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.stereotype.Component;

@Component
public class TennisCoach implements Coach {
	private FortuneService fortuneService;
	
//	@Autowired
//	public TennisCoach(FortuneService fs) {
//		fortuneService = fs;
//	}
	
	public FortuneService getFortuneService() {
		return fortuneService;
	}

	// Here is out setter method, autowired
	@Autowired
	public void setFortuneService(FortuneService fortuneService) {
		this.fortuneService = fortuneService;
	}
	
	@Override
	public String getDailyWorkout() {
		return "You can do it, Rafael";
	}


	@Override
	public String getDailyFortune() {
		return fortuneService.getFortune();
	}

}
```
No changes are needed to the main app, just run it and it'll be awesomely working ^_^

### Method Injection with Annotations
We can actually use ANY method, not just setters, to inject stuff.
Just add @Autowired to the method.

To test it, go to TennisCoach, just change the setter method's name to any other name.
Then move to the main app, run it, it'll work.

Spring is awesome, not as awesome as we are *obviously*, but it is awesome you know.
Not sure if it's awesome because we -the *most awesome peopl on the entire planet*- are using it, or it's awesome on its own B|

### Field Injection with Annotations
We can inject dependencies by setting values in our classes directly even private ones.
Behind the scenes, Spring does that by Injection.

Steps:
- Configure the dependency injection with @Autowired annotation.

That's it :'D

Just place @Autowired on the field directly
``` Java
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.stereotype.Component;

@Component
public class TennisCoach implements Coach {
	@Autowired
	private FortuneService fortuneService;
	
//	@Autowired
//	public TennisCoach(FortuneService fs) {
//		fortuneService = fs;
//	}
	
	public FortuneService getFortuneService() {
		return fortuneService;
	}

//	@Autowired
//	public void setFortuneService(FortuneService fortuneService) {
//		this.fortuneService = fortuneService;
//	}
	
	@Override
	public String getDailyWorkout() {
		return "You can do it, Rafael";
	}


	@Override
	public String getDailyFortune() {
		return fortuneService.getFortune();
	}

}
```

No changes to the main app, just run it and it'll be alive and kicking.

(I just thought that you may understand "kicking" as having som errors, but no, it means that it's working properly :'D )

### Which type of injection should I use?
Choose a style and stay consistent in your project i.e. in a project, choose ONE style and whenever you need injection, use THAT style.

People argue about which one is a superior style of injection, but you know that's just developers arguing, so choose the one you're comfortable with and do your thing.

### Qualifiers for Dependency Injection
So far we've been using Autowiring where:
- Spring scans components to find an implementation for FortuneService.
- Finds HappyFortuneService.
- And injects it.

What if, we have MORE THAN one implementation for FortuneService, like:
- RandomFortuneService.
- DatabaseFortuneService.
- RESTfulFortuneService.
- HappyFortuneService.

If you work normally like before, a NoUniqueBeanDefinitionException will be thrown
Along with a list of all implementations of what's being injected.

So, we have to qualify each one to be unique, using the @Qualifier annotation, passing a bean id to it (Be it default name or specified name).

Let's do that in code.
First, create a new class that implements for FortuneService, like RandomFortuneService.
``` Java
// Don't forget to add the @Component annotation
@Component
public class RandomFortuneService implements FortuneService {

	@Override
	public String getFortune() {
		return "And we keep driving through that night, it's a late goodbye..";
	}

}
```
Now run the main app.
The exception NoUniqueBeanDefinitionException will be thrown.

We'll modify TennisCoach and add the @Qualifier annotation, passing in the bean id we want, I'll pass the new one, randomFortuneService:
``` Java
@Component
public class TennisCoach implements Coach {
	
	// Notice here we passed the bean id of the bean we want
	@Autowired
	@Qualifier("randomFortuneService")
	private FortuneService fortuneService;
	
	public FortuneService getFortuneService() {
		return fortuneService;
	}
	
	@Override
	public String getDailyWorkout() {
		return "You can do it, Rafael";
	}


	@Override
	public String getDailyFortune() {
		return fortuneService.getFortune();
	}

}

```

Run the main app, it'll be working. <br />

***Note:***
In the default bean names, if the first AND second letters are uppercase, the name is NOT converted to camel case i.e. first letter stays uppercase.
That is done by a Spring component called *Java beans Introspector*, with a method called decapitalize.
Documentation:
https://docs.oracle.com/javase/8/docs/api/java/beans/Introspector.html#decapitalize(java.lang.String)

***Note:***
Weirdly, @Qualifier in constructors should be place WITH the arguments:
``` Java
@Autowired
public TennisCoach(@Qualifier("randomFortuneService") FortuneService theFortuneService) {        
    fortuneService = theFortuneService;
}
```

***Note:***
Injecting properties files using Java annotations is done by:
- Creating the file, obviously.
``` txt
foo.name=New York Giants
```
- Loading it into the config file, just like before.
``` xml
<context:property-placeholder location="classpath:sport.properties"/>
```
- Doing this in the class that has the fields to be injected:
``` Java
@Value("${foo.name}")
private String name;
```

## Bean Scope and Lifecycle with Annotations
### Bean Scope with Annotations
We simply use @Scope annotation to specify a bean's scope, passing in the scope we want, like @Scope("prototype") for example.

In eclipse, create a new main class called "AnnotationBeanScopeDemoApp", add the main code we've been writing for a while now:
- Load spring config file.
- Retrieving bean from container.
``` Java

public class AnnotationBeanScopeDemoApp {

	public static void main(String[] args) {
		ClassPathXmlApplicationContext context = 
			new ClassPathXmlApplicationContext("applicationContext.xml"); 
			
		Coach theCoach = context.getBean("tennisCoach", Coach.class);
		Coach alphaCoach = context.getBean("tennisCoach", Coach.class);
			
		System.out.println(theCoach == alphaCoach);
			
		context.close();
	}

}
```
If we run that, we'll get 'true' since the default scope is Singleton.

To change the scope, go to TennisCoach, add this to the class, above it:
``` Java
@Scope("prototype")
```
Since it's prototype, theCoach and alphaCoach should be different instances.

Run the main app, the result of comparing their equality will be "false".

Nice :) <br />


### Bean Lifecycle with Annotations
Steps:
- Define init and destroy methods.
- Add @PostConstruct and @PreDestroy annotations to the methods.

The same rules apply, the init and destroy methods are no-arg, can be of any return type but void is common, and can get any access modifier.

Go to TennisCoach, define init and destroy methods called doStartupStuff and doDestroyStuff or something.
Put some sysouts in them to verify that they do run.
**REMOVE THE PROTOTYPE SCOPE**, remember the destroy method is NOT called if the scope is prototype.
``` Java
@Component
public class TennisCoach implements Coach {
	
	@Autowired
	@Qualifier("randomFortuneService")
	private FortuneService fortuneService;
	
	public FortuneService getFortuneService() {
		return fortuneService;
	}
	
	@Override
	public String getDailyWorkout() {
		return "You can do it, Rafael";
	}


	@Override
	public String getDailyFortune() {
		return fortuneService.getFortune();
	}
	
	// Notice here the annotation
	@PostConstruct
	public void doStartupStuff() {
		System.out.println("Initializing the bean - custom mode");
	}
	
	// Notice here the annotation
	@PreDestroy
	public void doDestroyStuff() {
		System.out.println("Destroying the bean - custom mode");
	}

}

```
Run the main code without any changes to it, it'll work like crazy :)

***Note:***
If you're using Java 9 or higher, an error will be thrown for @PostConstruct and @PreDestroy annotations.

That's because they're part of javax.annotation, which has been removed from the default classpath, so Eclipse can't find it.

Solution:
1. Download javax.annotation:
http://central.maven.org/maven2/javax/annotation/javax.annotation-api/1.2/javax.annotation-api-1.2.jar

2. Copy the JAR file to the lib in your project.
3. Add it to the java build path.

## Spring Configuration With Java Code (no xml)
There are three ways to configure Spring container:
- Full xml config, the first thing we did.
- xml component scan, the second thing we did.
- Java config class, we'll do that now, we don't need to go to the xml at all.

Steps:
- Create a java class, annotate it as @Configuration.
- Add component scanning with @ComponentScan (optional)
- Read Spring Java config class.
- Retrieve bean from container.

### Java Config Class with Component Scaning

Create a new class, call it SportConfig.
Above it, add the @Configuration annotation.

The class is empty, so we either:
- Manually add beans and stuff.
- Use @ComponentScan annotation, passing in the package name.
	It will scan components and do the same stuff that it did before.

Here, Add the @ComponentScan annotation, pass to it the name of the package. i.e.  "com.luv2code.springdemo".


Create a new main class, call it "JavaConfigDemoApp", but instead of using ClassPathXmlApplicationContext to load an xml config file,
We use AnnotationConfigApplicationContext to load our SportConfig class.
``` Java
public class JavaConfigDemoApp {

	public static void main(String[] args) {
		AnnotationConfigApplicationContext context = 
			new AnnotationConfigApplicationContext(SportConfig.class); 
			
		Coach theCoach = context.getBean("tennisCoach", Coach.class);
			
		System.out.println(theCoach.getDailyWorkout());
		System.out.println(theCoach.getDailyFortune());
			
		context.close();
	}

}
```
Run it, and it'll work :) <br />

***Note:*** Log messages have been removed from latest Spring releases, so we only see our sysouts, not the Spring's logging.
To see the logging messages of Spring:
1. Create a logging properties file, in it define logger levels like this:
``` txt
root.logger.level=FINE
printed.logger.level=FINE
```
Read more about Logging here:
https://www.vogella.com/tutorials/Logging/article.html

2. Create a config class to config the parent logger and console handler, it will use @PropertySource annotation and @Value annotations to inject values into fields.
``` Java
import java.util.logging.ConsoleHandler;
import java.util.logging.Level;
import java.util.logging.Logger;
import java.util.logging.SimpleFormatter;
 
import javax.annotation.PostConstruct;
 
import org.springframework.beans.factory.annotation.Value;
import org.springframework.context.annotation.AnnotationConfigApplicationContext;
import org.springframework.context.annotation.Configuration;
import org.springframework.context.annotation.PropertySource;
 
@Configuration
@PropertySource("classpath:mylogger.properties")
public class MyLoggerConfig {
 
	@Value("${root.logger.level}")
	private String rootLoggerLevel;
 
	@Value("${printed.logger.level}")
	private String printedLoggerLevel;
	
	@PostConstruct
	public void initLogger() {
 
		// parse levels
		Level rootLevel = Level.parse(rootLoggerLevel);
		Level printedLevel = Level.parse(printedLoggerLevel);
		
		// get logger for app context
		Logger applicationContextLogger = Logger.getLogger(AnnotationConfigApplicationContext.class.getName());
 
		// get parent logger
		Logger loggerParent = applicationContextLogger.getParent();
 
		// set root logging level
		loggerParent.setLevel(rootLevel);
		
		// set up console handler
		ConsoleHandler consoleHandler = new ConsoleHandler();
		consoleHandler.setLevel(printedLevel);
		consoleHandler.setFormatter(new SimpleFormatter());
		
		// add handler to the logger
		loggerParent.addHandler(consoleHandler);
	}
	
}
```

### Java Config Class with Manual Bean Configuration
Steps:
- Define methods to expose the bean.
- Inject bean dependencies.
- Read java config class.
- Retrieve bean from Spring container.

First, we'll create a new implementation of FortuneService called SadFortuneService:
``` Java
public class SadFortuneService implements FortuneService {

	@Override
	public String getFortune() {
		return "Today is a sad day :(";
	}

}
```

Then create a new class SwimCoach that implements Coach. No special annotations or anything.
``` Java
public class SwimCoach implements Coach {
	private FortuneService fortuneService;
	
	public SwimCoach(FortuneService fs) {
		fortuneService = fs;
	}
	
	@Override
	public String getDailyWorkout() {
		return "Swim 1000 meters as a warmup";
	}

	@Override
	public String getDailyFortune() {
		return fortuneService.getFortune();
	}

}
```

#### Java Config Class with Manual Bean Configuration - Example
Go to SportConfig.
Add a method to define beans for SadFortuneService.
We'll use the @Bean annotation.
``` Java
@Bean
public FortuneService sadFortuneService() {
	return new SadFortuneService();
}
```
Then, add a method to define a bean for SwimCoach, and inject dependencies for SwimCoach.
``` Java
@Bean
public Coach swimCoach() {
	return new SwimCoach(sadFortuneService());
	// Notice we call the previous method here to inject dependencies
}
```
Remove the @ComponentScan annotation.
The class looks like this now:
``` java
@Configuration
public class SportConfig {

	@Bean
	public FortuneService sadFortuneService() {
		return new SadFortuneService();
	}
	
	@Bean
	public Coach swimCoach() {
		return new SwimCoach(sadFortuneService());
	}
	
}
```
You can name the methods anything, but you'll use thise names as bean id after that.

Now, make a new main app called "SwimJavaConfigDemoApp".
``` Java
public class SwimJavaConfigDemoApp {

	public static void main(String[] args) {
		AnnotationConfigApplicationContext context = 
			new AnnotationConfigApplicationContext(SportConfig.class); 
				
		// Notice here we used the same name as the method
		// that makes the bean, and the dependencies are injected
		Coach theCoach = context.getBean("swimCoach", Coach.class);
				
		System.out.println(theCoach.getDailyWorkout());
		System.out.println(theCoach.getDailyFortune());
				
		context.close();
	}

}
```

***Note:***
How does @Bean work?
- First, the @Bean annotation intercepts the initial request for "swimCoach" bean.
- Spring scans the memory for an instance of swimCoach.
- If none found, then:
	- The annotation tells Spring that we're creating a Bean component manually.
	- No scope is defined, so default is singleton.
	- The method name will be the bean ID.
	- The return type is of an interface, this is useful for injection for something that has many implementations.
- The @Bean annotation will intercept any requests for "swimCoach" bean after that, because the scope is Singleton so it returns the same instance of the bean that was created before, effectively setting a flag.
- So, if we call the method again, the code inside it doesn't actually execute, the reference to the created instance will instantly be returned.
- The same process for sadFortuneService.

In short, methods that define beans execute only once if the scope is Singleton.

### Java Config Class with injecting Values by a Properties File
Steps:
- Create a properties file.
- Load it in Java Config Class.
- Reference its values.

#### Step 1: Create a properties file
Create a properties file and put in it:
``` txt
foo.email=moamen.is.awesome@gmail.com
foo.team=New York Giants
```
#### Step 2: Load it in Java Config Class
Then go to SportConfig, use @PropertySource on the class and pass "classpath:sport.properties" to it as argument.
``` Java
@Configuration
//@ComponentScan("com.luv2code.springdemo")
@PropertySource("classpath:sport-properties")
public class SportConfig {

	@Bean
	public FortuneService sadFortuneService() {
		return new SadFortuneService();
	}
	
	@Bean
	public Coach swimCoach() {
		return new SwimCoach(sadFortuneService());
	}
	
}
```
#### Step 3: Reference its values
Go to SwimCoach, 
Create email and team fields,
Use the @Value annotation to inject values into fields.
Also, generate the getters for the new fields, I generated both getters and setters.
``` Java
public class SwimCoach implements Coach {
	private FortuneService fortuneService;

	@Value("${foo.email}")
	private String email;
	
	@Value("${foo.team}")
	private String team;
	
	public String getEmail() {
		return email;
	}

	public void setEmail(String email) {
		this.email = email;
	}

	public String getTeam() {
		return team;
	}

	public void setTeam(String team) {
		this.team = team;
	}

	public SwimCoach(FortuneService fs) {
		fortuneService = fs;
	}
	
	@Override
	public String getDailyWorkout() {
		return "Swim 1000 meters as a warmup";
	}

	@Override
	public String getDailyFortune() {
		return fortuneService.getFortune();
	}

}
```
Now, we go to the main app, but change Coach to SwimCoach so that we can use the new fields, getters, and setters.
``` Java
public class SwimJavaConfigDemoApp {

	public static void main(String[] args) {
		AnnotationConfigApplicationContext context = 
			new AnnotationConfigApplicationContext(SportConfig.class); 
				
		SwimCoach theCoach = context.getBean("swimCoach", SwimCoach.class);
				
		System.out.println(theCoach.getDailyWorkout());
		System.out.println(theCoach.getDailyFortune());
		
		System.out.println(theCoach.getEmail());
		System.out.println(theCoach.getTeam());
				
		context.close();
	}

}
```

Run that awesomeness, and it will work like The Miz (WWE reference xD).

***Note:***
If the output was:
${foo.email}
${foo.team}
instead of their values, that's a problem with Spring versions.
If you're using Spring 4.2 or lower, you need to add this code to SportConfig:
``` Java
 // add support to resolve ${...} properties
    @Bean
    public static PropertySourcesPlaceholderConfigurer
                    propertySourcesPlaceHolderConfigurer() {
        
        return new PropertySourcesPlaceholderConfigurer();
    }
```

## Spring MVC - Building Spring Web Apps
### What is Spring MVC?
- A framework for building Web apps in Java.
- Based on MVC.
- Leverages Core Spring framework.

The full documentation of Spring MVC (Which is very good btw):
https://docs.spring.io/spring/docs/current/spring-framework-reference/web.html

Books:
Just search Spring MVC on amazon, you'll find a lot of good books.

### Components of Spring MVC
- A set of web pages to layout UI components.
- A collection of Spring Beans.
- Spring configurations.

So how does Spring MVC work behind the scenes?
- An incoming request occurs.
- A component called "Front Controller" or "DispatcherServlet" that will delegate the request to other objects in our system.
- It's part of the Spring framework.
- Already developed and in the JAR files we loaded.
- WE create the Model, View, and Controller classes.
	- Model: contain data.
	- View: the JSP page where data is rendered.
	- Controller: The business logic.
	
Controller:
- This is the business logic, here we handle requests, store/retrieve data, place them in model, etc.
- Send that to the appropriate view template.

Model:
- The local container that contains the data.
- Stores and retrieve data from databases or web services.
- It can be any java object or collection.

View:
- View templates and model data, where our JSP page reads the model data and displays it.
- Most commonly is JSP + JSTL.
- There are other view templates like Groovy, Velocity, Thymeleaf, Freemarker, etc. Check the docs.

### Setting Up Spring MVC
We already did these:
- Install Tomcat.
- Install Eclipse.
- Connect Eclipse to Tomcat.

Now we'll do two more steps:
- Download Spring MVC source code for this training.
http://www.luv2code.com/downloads/udemy-spring-hibernate/solution-code-spring-mvc-config-files.zip
- Download the latest Spring JAR viles from the Spring website.

### Spring MVC Configuration
This is probably the hardest part of Spring MVC: getting the config right.

Steps:
- Add configurations to the file WEB-INF/web.xml
	- Configure Spring MVC Dispatcher Servlet.
	- Set up URL mappings to that Servlet.
- Add configurations to file WEB-INF/spring-mvc-demo-servlet.xml
	- Add support for Spring component scanning, conversion, formatting, valudation.
	- Configure Spring MVC View Resolver.

Change your Eclipse perspective:
Open the Window menu -> Perspective -> Open Perspective -> Java EE

Create a new project:
File -> New -> Dynamic Web Project
Call it "spring-mvc-demo"
In WebContent/WEB-INF/lib, add the spring JAR files, along with the JAR files from the latest download, the ones in this path:
solution-code-spring-mvc-config-files\solution-code-spring-mvc-config-files\spring-mvc\starter-files\spring-mvc-demo\lib

Then, in the same path:
solution-code-spring-mvc-config-files\solution-code-spring-mvc-config-files\spring-mvc\starter-files\spring-mvc-demo\config

Get: 
- web.xml 
- spring-mvc-demo-servlet.xml
And put them in WEB-INF/lib

***Note:*** These files usually cause a lot of problems since configuring them isn't easy, but these ones are pre-configured, use them until you get good at configuring them.

Open web.xml, you'll find the steps we discussed earlier.
Open spring-mvc-demo-servlet.xml, which is a regular config file for Spring, also the configs we discussed earlier are viewed here.

Create the folder 'view' in WEB-INF.

Now Spring MVC should be configured and ready to go, we'll start writing code next.

### Spring MVC: Creating Controllers and Views
Steps:
- Create controller class.
- Define controller method.
- Add request mapping to the controller method.
- Return the view name.
- Develop view page.

We'll use @Controller annotation, which inherits from @Component, so it will be picked up during component scanning.

#### Step 1: Create controller class
Create a new package in Java Resources/src, call it "com.luv2code.springdemo.mvc".
Create a new class, called "HomeController".
Add the @Controller to the class
``` Java
@Controller
public class HomeController {

}
```
#### Step 2: Define controller method
Add a new method to the controller:
``` Java
@Controller
public class HomeController {
	public String showPage() {
		return null;
	}
}
```
#### Step 3: Add request mapping to the controller method
Add the annotation @RequestMapping and pass "\" as argument:
``` Java
@Controller
public class HomeController {
	
	@RequestMapping("/")
	public String showPage() {
		return null;
	}
}
```
#### Step 4: Return the view name
Return the name of the page (the incomplete name of the page because the prefix and suffix are added from spring-mvc-demo-servlet.xml )
``` Java
@Controller
public class HomeController {
	
	@RequestMapping("/")
	public String showPage() {
		return "main-menu";
	}
}

```
#### Step 5: Develop view page
In the WebContent/WEB-INF/view folder, create a new file, name it <the-view-name-you-returned-in-last-step>.jsp, here it's "main-menu.jsp".
Inside it, write any html you want like:
``` html
<!DOCTYPE html>

<head>
	<title>Moamen's Spring MVC</title>
</head>

<body>
	<span>Moamen is amazing</span>
</body>
```

Now right-click on the project folder, Run as Run on server, choose default server, click finish.