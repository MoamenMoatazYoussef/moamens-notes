# Java Enterprise Edition

## Table of Contents

## Prerequisites

## Tips

## Let's Get The Show on The Road
### Basics of basics
To dive in the world of web development, enterprise development, and Java Enterprise Edition (or Java EE) in general, we'll need to explore the software industry a little bit.

Basically there are two types of applications:
- **Consumer applications**: these are applications that are used by *consumers*, the people who use apps for their daily life i.e. me, you, us, examples are:
    + Any app on your iphone, that app is designed for you, the consumer, to help you make life better.
    + Any app you use on your computer, like Browsers, Office tools, and even games.
- **Enterprise applications**: these are applications used by *businesses*, to make the business conduct its daily operations in an efficient way, examples are:
    + An app that employees use to request vacations from HR.

**What's the difference between the two?** <br/>
Let's take an example of each application type: the game *The Evil Within 2* as a consumer app, and *an HR system* as an enterprise app.

The Evil Within 2 is a game, an app used by a consumer, usually one at a time (forget multiplayer for now), so it should do some things, for example:
- It should run on the consumer's computer, whatever its specs.
- It should save the consumer's data and load it on demand (like saving the game and loading it).

The first thing is achieved by bundling the game with all of the things it needs to run, like libraries, etc. (These are called Dependencies).

The second thing is achieved by accessing the file system, creating a file, and writing some data in a format that can be read back by the game and understood. (This is called Persistence)

Notice that this app didn't need a database, or network, or to communicate with other computers or the internet, it's a standalone game that works on your computer, that's it. (Again, forget multiplayer and DLCs for now).

**Will the *HR system* be the same?**
No, it will be different, let's look at it:
- The HR system is used by HR people and other employees.
- Employees can open the app to request vacations and log in their working hours.
- HR people can open the app to see the employees' performance and vacation requests.
- HR people can also approve or reject the vacation requests.
- Also, the employee's manager can also view his performance and vacation requests and can approve or reject them.

So, the app should do some things:
- It should send a notification to HR and managers when an employee requests a vacation, and the employee should receive a notification when the HR and manager approve or reject the request.
- The employee should see a user interface that lets him only request vacations and log in hours, but not approve or reject vacations.
- The same applies for HR and managers, their UI should let them approve/reject vacations.
- Vacations are limited, and are renewed each year, so the application should know that the employee has had a vacation.
- And it should renew vacations each year.

Okay hold on, this is a lot of stuff xD
- Employees, HR, and managers open the app from different computers, so we need the app to use a *network*.
- Each user interface is different, so the app should know who is opening it, so we need to *authenticate* whoever's using the app.
- When the user does something (requests or approves/rejects a vacation), this should be sent over the network to the app so that the app can do whatever's needed, 
    + since this is like logic that runs when a user requests it, it's called a *service*, 
    + and it's conducted over a network, so it's called a *web service*, 
    + so, the app should provide *web services* for the user to request
    + the user requests them using *URLs*.
- The app should have a database for vacations of all employees and communicate with it when anyone requests vacations.

That's a lot of stuff, but they all mean that enterprise applications are a bit different from normal applications.


Okay, but here's a question:
The consumer app *The Evil Within 2* runs on the user's PC. **Where does the HR app run?** <br/>
The code that has the core logic of the HR application runs not on any employee's PC, it runs on a **Server**, dedicated to run this app. 

The code that runs on that server is called an *Application Server*.

The *Application Server* is the concrete logic of the application.

We'll learn how to developer enterprise apps and application servers using *Java Enterprise Edition*, or ***Java EE***.

### Overview of Java EE
**So what the hell is Java EE?** <br/>
Java EE is two things:
- A set of guidelines (or specifications) and APIs that should be followed by developers so that they can develop good enterprise applications.
- A set of APIs that we can use to follow these guidelines. <br/>

Every software application solves problems, some of these problems are common and encountered by most if not all developers, examples of this are:
- Database access
- Persistence (saving and loading data in local caches)
- Web services
- Application security
Java EE provides ready APIs to handle all of these problems. <br/>

That is why Java EE is **Abstract**, because you, the developer, use the APIs without needing to know how they work.

Also, Java EE is **Portable**, because the guidelines or specs provided by Java EE are interfaces that we implement, so our app can run on *any* Java EE application server.

Examples of these APIs are:
- *javax* package: this is a package that contains all the standard interfaces that we'll use and implement to develop applications.
- *javax.persistence* package: it's a subset of *javax*, it's a collection of interfaces that is used to communicate with a Database.

In Java EE, an *Application Server* refers to a concrete implementation of Java EE interfaces, some Application Servers are developed by other people and can be downloaded and used to run our Java EE code, some of them are:
- Payara Server: https://payara.fish
- IBM OpenLiberty: https://openliberty.io
- JBoss Wildfly: http://www.wildfly.org/

We'll download one of them, and add it to our IDE, then develop our code normally, then tell the IDE to use the server to run our code.

Remember that java EE is **abstract** and **portable**, meaning we can develop on Payara server, and run on JBoss server, without knowing how each works, and without changing anything in our Java EE code.

**But, does Java EE cover every problem?** <br/>
No, and to solve that problem,other people can enhance Java EE and add things to it. <br/>

There is a governing body for Java EE, called the *Java Community Process*, or *JCP*, this "java government" receives other people's proposals for improvements in Java EE, reviews them, and if they are accepted, they are included in the next Java EE release.

The proposals made by other people to the JCP is called a *Java Specification Request*, or *JSR*.

You can view some example JSRs here in the JCP website:
https://www.jcp.org/en/home/index

For example, to develop RESTful web services (web services that conform to the REST standards), you can use the JAX-RS request:
https://jcp.org/aboutJava/communityprocess/final/jsr370/index.html

There is a document in that link, if you develop code that follows the standards written in the document, your code is going to be portable and work on any Application Server that implements the JAX-RS request.

Remember that:
- Jave EE is a set of specifications i.e. a set of Java Interfaces.
- a JSR is an improvement of these specifications i.e. new interfaces or improved interfaces added to Java EE.
- So, JSRs are also interfaces, abstract interfaces that need to be implemented to be used.

So, to use a JSR, we need to have a concrete implementation of a JSR to be able to use the interfaces of the JSR.

**How do we get an implementation for a JSR?** <br/>
There is a implementations for each JSR, specificly made so that other developers who want to use the JSR, can get a ready-made implementation.

This implementation is called a **Reference Implementation**, and it's shipped with every JSR.

***Note:*** by this definition, Java EE is itself a JSR, for example, Java EE 8 is actually the 366th request to improve Java EE, or JSR 366.
It has a **Reference Implementation**, which is RI Glassfish 5.

Some JSRs have different implementations, for example the *javax.persistence* package (Also known as *Java Persistence API*, or ***JPA***), has many reference implementations like:
- EclipseLink.
- Hibernate.

**How is Java EE improving?** <br/>
Oracle develops Java EE, and Oracle decided to open up the development of Java EE to the wide software community, so they gave Java EE to the Eclipse Foundation (The company that made Eclipse IDE), since this community supports open-source development (for example, Eclipse IDE itself is open source).

So, in the future, Java EE will be released by Eclipse Foundation, and it will be called **Jakarta EE**.

**Jakarta EE** is an upgrade of Java EE i.e. you will still use your Java EE skills to use Jakarta EE,

**What about Spring Framework?** <br/>
Spring is a gigantic Java framework that is influenced by Java EE. It's made to solve common Java EE problems and also make development much simpler.

At its beginning, it was a better version of Java EE, but Java EE is also progressing a lot.

A lot of people get caught up in the conflict of which is better: Spring or Java EE. <br/>

In business, they are competing against each other, but in development, both are very powerful, there is little things that can be done by one and can't be done by the other.

So, use whatever works for you.

***Note from Moamen:*** I already studied Spring and I specialize in it, there are a lot of scripts of Spring that takes you step by step to learn them, check them out at:
http://bit.ly/31cil0i


**Recap** <br/>
- Java EE is a set of specifications that we should follow to develop good Enterprise applications.
- Java EE is updated with requsts and proposals from other people, called JSRs.
- Java EE will be named Jakarta EE in the future and will be released by Eclipse Foundation, not Oracle.
- Spring Framework is a very popular and the primary competitor of Java EE, both are very powerful.

## Setting Up for Java EE Development
Before we start learning Java EE, we'll need to set up our environment with some tools:
- Java JDK 8 at least.
- An IDE: we'll use NetBeans and IntelliJ.
- Maven.
- Git.
- Insomnia REST client (it's simplar to Postman).
- MySql Database Installed
- 

***Note from Moamen:***
- Check out the Maven tutorial here:
http://bit.ly/2VGT7Gj
- Check out the MySql installation instructions here:

- 