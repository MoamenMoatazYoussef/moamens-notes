# Hibernate
## Table of Contents

## Notes
- sysout means System.out.println, I'm lazy.
- DB means Database.
- ORM means Object-to-Relational Mapping.

## What is Hibernate anyway?
- It's a framework for persisting and saving Java objects into a database.
- It's a very popularframework.
- It handles low-level SQL queries.
- It minimizes the amount of JDBC code we need to write.
- It provides object-to-relational mapping (ORM).

## How does it work?
- We need to tell Hibernate how an object or class maps to database.
- We set 1-to-1 mapping between fields in the class/object and columns in the database:
    - via a config file using xml, or
    - using Java annotations.
- We'll map our objects and classes to a database table.

Example:
``` Java
Student theStudent = new Student("Moamen", "Moataz", "moamen.moataz.youssef@gmail.com");

// Saving data into database
int theID = (Integer) session.save(theStudent);
//this:

// Retrieving data from database
Student myStudent = session.get(Student.class, theID);
```
How does saving to database work?
- In the background hibernate takes that object based on those mappings defined earlier, and store it in the database appropriately.
- This is actually an SQL insert.

How does retrieving from database work?
- When we save something in the database in Hiberate, it returns the PRIMARY KEY of that record in the database (which is the value returned in theID).
- So, to retrieve the data we use theID.

If we want to return a list of Student objects, we do this:
``` Java
Query query = session.createQuery("from Student");
List<Student> students = query.list();
```
- This is equivalent to a query like SELECT * FROM STUDENT in SQL.

## What is the relationship between Hibernate and JDBC?
- What is JDBC?
- Java DataBase Connectivity, an API for Java to specify how to access a database.

- Hibernate actually USES JDBC for all DB communications.
- It's another layer on top of JDBC.
-So, all the low-level is JDBC, all the high-level is Hibernate.
- It's like syntatic sugar for JDBC, but not really.

## Hibernate Installation
Steps:
1. Install JDK 8 or higher.
2. Install a Java IDE (eclipse for exmaple).
3. Set up a Database server (MySQL for example).
4. Set up Hibernate in Eclipse.

Step 1 and 2 are covered in Spring notes, check them out.

**Step 3: Set up MySQL**<br/>
Installation:
- Go to dev.mysql.com/downloads.
- Choose MySQL Community Server.
- Download MySQL installer.
- Select the appropriate option from here, like the msi Web version, a small light-way version.
- Start the download (forget the signup).
- It's 18 MB so it'll take seconds (or days if you're TE data).
- Start the installer.
- Next.
- Next (Or choose developer defaults if not chosen already).
- Next, they'll list the requirements for MySQL server, choose what you don't have and the installer will download an install what you need.
- Next, execute.
- it'll start downloading all dependencies you need.
- Next, next.
- Set a password, make sure to remember that.
- Next, next.
- Execute. (It'll run the MySQL server)
- Finish.
- Use the password to check that the server is OK.
- Next, execute, finish, next, finish.

Verification:
- Now to verify installation, open MySQL Workbench, it's a GUI tool to connect to the DB and query it and stuff.
- In MySQL connections, you can create a connection or choose an existing one.
- Choose a connection , enter password.
- See if it opens.
- If there's any tables in the database, try querying them using the Workbench.
- That's it, you verified the installation :)

***Note:*** Official MySQL installation instructions:
https://dev.mysql.com/doc/refman/5.7/en/osx-installation.html

Setting up Database tables:
- In the source code download of this course (hibernate-sql-scripts-and-starter-files.zip), you'll find two files:
    - 01-create-user.sql.
        This is used to create a new MySQL user for our application so that Hibernate uses it to communicate with the database:
            - user: hbstudent
            - pass: hbstudent
    - 02-student-tracker.sql.
        This script creates a new Student table which has fields:
        - id. (primary key)
        - first_name.
        - last_name.
        - email.

To execute these files, read them in the Workbench then execute them:
- Click on the folder icon, it'll browse.
- Open 01-create-user.sql.
- It'll open the actual SQL query written in the file.
- Click on the "lightning bolt" icon to execute that query.
- In the Output section on the bottom, the status of the query will be shown.
- To verify that the user has been created, close the connection in the workbench, make a new connection, click Test connection, put in user and pass as hbstudent, execute and see the result.
- Then log in to the connection with that user.


To execute the second script:
- File menu -> Open SQL Script.
- Choose the second script, obviously.
- THe script basically creates a database if not exists, deletes table student if exists, and creates a new one.
- Execute it.
- A warning flag will appear, that's normal, as long as you have the green checkmarks, you're good to go.

Refresh:
- On the left section, on its bottom, a tab called schema, click it.
- Right-click on any space inside that, click Refresh all.

**Step 4: Set up Hibernate in Eclipse** <br />
Set up Eclipse for Hibernate:
- Open Eclipse, change the perspective to Java.
- Create a new Java project, call it hibernate-tutorial, then Finish.
- Create a new folder in the project directly, right-click and new folder, call it lib.

Download Hibernate JAR files and JDBC driver.
- Go to www.hibernate.org.
- Choose Hibernate ORM.
- Click Download.
- Scroll to version 5.4 or latest stable one, choose Download.
- It's 64 MB, so it might take a couple of minutes.
- Extract the download file.
- Go into it, to lib directory, you'll find a number of folders:
    - required: the minimum requirements of Hibernate, must be in any project working with Hibernate. (Copy them to your project's lib folder)

Downloading Hibernate JDBC driver:
- dev.mysql.com
- Downloads.
- "Community" tab.
- Scroll down until you find "MySQL Connectors" and choose it.
- Choose Connector/J.
- Select "Platform Independent".
- Then click Download for platform ZIP file.
- It's 4 MB, should take an instant.
- Unzip it.
- Inside, you'll find one jar file, copy it to your project's lib.
- Finally, add the jar files now in your project's lib to the build path.

Done with the setup, yo! <br/>

**Testing JDBC Connection**<br/>
- Create a new package called "com.luv2code.jdbc".
- Create a new class called "TestJDBC", include public static void main method.
- Write this code:
``` Java
package com.luv2code.jdbc;

import java.sql.Connection;
import java.sql.DriverManager;

public class TestJDBC {

	public static void main(String[] args) {
		String jdbcUrl = "jdbc:mysql://localhost:3306/hb_student_tracker?useSSL=false&serverTimezone=UTC";
		// for MySQL lower than 8, remove the serverTimezone argument from the url.
		// the useSSL=false is to suppress any warnings for now.
		
		String user = "hbstudent";
		String pass = "hbstudent";
		
		try {
			System.out.println("Connecting to: " + jdbcUrl);
			
			Connection myConn = DriverManager.getConnection(jdbcUrl, user, pass);
			
			System.out.println("Connection Successful!");
			
		} catch(Exception e) {
			System.out.println("Connection Failed, cause:\n");
			e.printStackTrace();
		}
	}
}

```
- Right-click in the file, run as, java application.

Nice. :) <br/>

***Note:*** You can change the password to see if it rejects the connection to further make sure it works.

## Hibernate Configuratin With Annotations
### Hibernate Configuration
- Hibernate has a config file that specifies how Hibernate connects to the database.
- The config file will have the JDBC configuration (user, password, url, etc.).
- The config file will have other smaller config items.

**Step 1: Add the Hibernate config file** <br/>
- See the Hibernate source files for this course, go to starter-files.
- You'll fine ONE file, hibernate.cfg.xml.
- Copy it into your eclipse project's src directory, so that it's available for Hibernate to use it. since it has to be on the CLASSPATH of your app.
- You can put it in other locations then include it in the build path, but this is the simple approach.
- Open the file.

What can we see here? <br/>
- Some parameters set up for "session-factory", which is basically a factory that returns session objects for us to connect to the DB.
- The JDBC Database connection settings is the most important thing here.
- It has driver_class, url, username, password, this tells Hibernate how to connect to our MySQL DB.
- Then, a JDBC connection pool, we'll just set it to 1 (Already done).
- SQL dialect:
    - SQL is a standard, but it has different "dialects" or "forms", like Oracle, SQL Server, Postgres, etc.
    - We just set it to whatever dialect we're using, which here is MySQLDialect (you don't need to set it).
- Echo SQL to stdout: makes SQL log the SQL it's going to use when it communicates with the DB, useful for testing.
- current session context class: which dev model we'll use, here it's thread, so we're using the threaded model.

### Annotate the Java Class
In Hibernate, there's something called Entity Class:
- A Java Class that's mapped to a DB table.
- It's a very normal plain old java class, fields, getters, setters, etc.
- But it has some special annotations on it, that maps it to the DB. (This is the ORM we talked about earlier)

There are TWO options for mapping a Java Class:
- XML config file (Legacy approach, but still available)
- Java Annotations (Preferred)

We'll only cover the preferred way, if you want to check out the XML way search the internet.

So, how do we map a class using Annotations?
+ First, map the class to a table:
    - Annotate the class by @Entity.
    - Annotate it again by @Table(name="table_name"), passing in the name of the DB TABLE you'll map the class to.
+ Second, map the class fields to the table's columns:
    - Annotate the id field by @Id (which makes it the PRIMARY KEY of each instance).
    - Then annotate the id again with @Column(name="column_name") passing in the name of the Column you'll map the field to.
    - For all other fields, only use the @Column(name="column_name") annotation.

***Note:***
- If you are using Java 9, 10 or 11, then you will encounter an error when you run your Hibernate program.
- These are the steps to resolve it. Come back to the lecture if you hit the error.
``` Java
Error: Exception in thread "main" java.lang.NoClassDefFoundError: javax/xml/bind/JAXBException
```
- This happens because of Java 9 and higher. 
- Java 9 and higher has removed java.xml.bind from its default classpath. That's why we get the class not found exception.  We have to explicitly add JAR files to the build path.

**Solution**
- For Java 9 and higher, you need to additional JAR files.\
1. You need to download the following JAR files, so download them:
    + javax.activation-1.2.0.jar: http://search.maven.org/remotecontent?filepath=com/sun/activation/javax.activation/1.2.0/javax.activation-1.2.0.jar
    + jaxb-api-2.3.0.jar: http://search.maven.org/remotecontent?filepath=javax/xml/bind/jaxb-api/2.3.0/jaxb-api-2.3.0.jar
    + jaxb-core-2.3.0.jar: http://search.maven.org/remotecontent?filepath=com/sun/xml/bind/jaxb-core/2.3.0/jaxb-core-2.3.0.jar
    + jaxb-impl-2.3.0.jar: http://search.maven.org/remotecontent?filepath=com/sun/xml/bind/jaxb-impl/2.3.0/jaxb-impl-2.3.0.jar

2. Copy the JAR files to the lib folder of your project.
3. Add them to your Build Path.

- Eclipse will perform a rebuild of your project and it will resolve the related build/runtime errors.
``` Java
Error: import of the javax.persistance.GenerationType saying its not accessible
```
- You may still encounter problems for "import of the javax.persistance.GenerationType saying its not accessible"

- To resolve this:
1. Right Click on the Project -> Properties - > Java Build Path.
2. Follow the steps as showed in the images below.
3. In Module Properties -> Select All in Available modules and move to Explicitly included modules.
4. Project->Clean... and Rebuild the Project and try again.

***Note:*** if you are using Maven then you can add this to your POM file
``` xml
<dependency>
   <groupId>javax.xml.bind</groupId>
   <artifactId>jaxb-api</artifactId>
   <version>2.2.8</version>
</dependency>

<dependency>
   <groupId>com.sun.xml.bind</groupId>
   <artifactId>jaxb-core</artifactId>
   <version>2.2.8</version>
</dependency>

<dependency>
   <groupId>com.sun.xml.bind</groupId>
   <artifactId>jaxb-impl</artifactId>
   <version>2.2.8</version>
</dependency>

<dependency>
   <groupId>com.sun.activation</groupId>
   <artifactId>javax.activation</artifactId>
   <version>1.2.0</version>
</dependency>
```

***Note:***
For Java 9 users only:
- The Generate toString() process fails in Eclipse with Java 9.
- When clicking on Generate toString() an error message pops up saying:-
``` txt
Cannot Create method Implementations

Reason:

C:\Program Files\Java\jre 9.04\lib\jrt-fs.jar\java.base[java.base is not in project's build path]
```
- This is a bug in Eclipse when using Java 9.
- It is a known issue and logged here by the Eclipse team.
- Bug link: https://bugs.eclipse.org/bugs/show_bug.cgi?id=521995
- As a work around, you will need to manually write the code for toString(). You can move ahead in the video and then pause the video where you see the toString() code.

**Step 1: Annotate the Java Class**
- Create a new package called "com.luv2code.hibernate.demo.entity".
- Create a new class in it, called "Student".
- Annotate it with:
    - @Entity
    - @Table(name="student")
- To fix imports, right-click in the file, Source, Organize Imports, select javax.persistence.Entity and javax.persistence.Table.
- Create a no-arg constructor.
- Add the fields.
``` Java
import javax.persistence.Entity;
import javax.persistence.Table;

@Entity
@Table(name="student")
public class Student {
	private int id;
	private String firstName;
	private String lastName;
	private String email;
	
	public Student() {
		
	}
}
```
**Step 2: Annotate the Class Fields**
- Annotate the id with:
    - @Id
    - @Column(name="id")
- Fix the imports, also choose javax.persistence stuff.
- Annotate the rest of the fields with @Column(name="first_name"), @Column(name="last_name"), and @Column(name="email").
``` Java
import javax.persistence.Column;
import javax.persistence.Entity;
import javax.persistence.Id;
import javax.persistence.Table;

@Entity
@Table(name="student")
public class Student {
	
	@Id
	@Column(name="id")
	private int id;
	
	@Column(name="first_name")
	private String firstName;
	
	@Column(name="last_name")
	private String lastName;
	
	@Column(name="email")
	private String email;
	
	public Student() {
		
	}
}
```
- Add another constructor cuz we'll use it later: 
    - right-click on file.
        -> source
        -> Generate Constructor Using Fields.
    - Select all fields except id.
    - OK.
- Generate getters and setters for all fields.
- Finally, we'll add toString method for debugging, right-click -> source -> generate toString(), select all fields but no methods, OK.
- Here's the class:
``` Java
@Entity
@Table(name="student")
public class Student {
	
	public int getId() {
		return id;
	}

	public void setId(int id) {
		this.id = id;
	}

	public String getFirstName() {
		return firstName;
	}

	@Override
	public String toString() {
		return "Student [id=" + id + ", firstName=" + firstName + ", lastName=" + lastName + ", email=" + email + "]";
	}

	public void setFirstName(String firstName) {
		this.firstName = firstName;
	}

	public String getLastName() {
		return lastName;
	}

	public void setLastName(String lastName) {
		this.lastName = lastName;
	}

	public String getEmail() {
		return email;
	}

	public void setEmail(String email) {
		this.email = email;
	}

	@Id
	@Column(name="id")
	private int id;
	
	@Column(name="first_name")
	private String firstName;
	
	@Column(name="last_name")
	private String lastName;
	
	@Column(name="email")
	private String email;
	
	public Student() {
		
	}

	public Student(String firstName, String lastName, String email) {
		super();
		this.firstName = firstName;
		this.lastName = lastName;
		this.email = email;
	}
}
```

***Note:***
There is a Java Persistence API or JPA already, why use Hibernate? <br/>
- JPA is a standard specification.
- Hibernate is an implementation of JPA.
- Hibernate team actually recommends the use of JPA annotations as a best practice.

## Hibernate CRUD Features
### Creating and Saving Java Objects
Hibernate has two key players:
- SessionFactory: 
    + reads Hibernate config file, creates Session objects, and returns them.
    + Heavyweight object: created ONCE in our app, and reused.
- Session:
    + A wrapper around a JDBC connection to DB.
    + The object we'll use to save and retrieve objects to and from the DB.
    +  A one-time object, in each method, we get a session, use, and throw it away.

Example of using both:
``` Java
public static void main(String[] args) {
    SessionFactory factory = new Configuration()
                                .configure("hibernate.cfg.xml")
                                .addAnnotatedClass(Student.class)
                                .buildSessionFactory();
    
    // The config filename isn't required,
    // we can just say .configure() because the name
    // hibernate.cfg.xml is the default, and Hibernate
    // will look for it in the classpath

    Session session = factory.getCurrentSession();
    try {
        // use session to save/retrieve data

        // for example, 
        // Saving:
        Student tempStudent = new Student("M", "M", "M@m.com");
        
        // starting a transaction
        session.beginTransaction();

        // saving the tempStudent object to the DB
        // Hibernate knows how to connect to DB and
        // how to map tempStudent to the DB
        session.save(tempStudent);

        // commit the transaction
        session.getTransaction().commit();

    } catch(...) {
        // your catch blocks here.
    } finally {
        factory.close();
    }
}
```
