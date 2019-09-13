# Hibernate
## Table of Contents
- [Notes](#notes)
- [What is Hibernate anyway?](#what-is-hibernate-anyway-)
- [How does it work?](#how-does-it-work-)
- [What is the relationship between Hibernate and JDBC?](#what-is-the-relationship-between-hibernate-and-jdbc-)
- [Hibernate Installation](#hibernate-installation)
- [Hibernate Configuratin With Annotations](#hibernate-configuratin-with-annotations)
  * [Hibernate Configuration](#hibernate-configuration)
  * [Annotate the Java Class](#annotate-the-java-class)
- [Hibernate CRUD Features](#hibernate-crud-features)
  * [Creating and Saving Java Objects](#creating-and-saving-java-objects)

<small><i><a href='http://ecotrust-canada.github.io/markdown-toc/'>Table of contents generated with markdown-toc</a></i></small>

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

**Let's try this in code**
- Create a new class called "CreateStudentDemo" and in the package field, write "com.luv2code.hibernate.demo", a new package will be created with the class in it.
- Make sure to import the4 Student class in the com.luv2code.hibernate.demo.entity package.
- Create a session factory.
- Use it to create a new session.
- Inside a try catch finally block:
    - Try: create a student, start a transaction, save the object, commit the transaction.
    - Catch.
    - Finally: Close the session factory.
``` Java
import org.hibernate.Session;
import org.hibernate.SessionFactory;
import org.hibernate.cfg.Configuration;

import com.luv2code.hibernate.demo.entity.Student;

public class CreateStudentDemo {

	public static void main(String[] args) {
		SessionFactory factory = new Configuration()
				.configure("hibernate.cfg.xml")
				.addAnnotatedClass(Student.class)
				.buildSessionFactory();
		
		Session session = factory.getCurrentSession();
		
		try {
			// "Creating student object..."
			Student s = new Student("Moamen", "Moataz", "moamen.moataz.youssef@gmail.com");
			
			// "Starting transaction..."
			session.beginTransaction();
			
			// "Saving the object..."
			session.save(s);
			
			// "Commit the transaction..."
			session.getTransaction().commit();
			
			// "Done."
		} catch(Exception e) {
			e.printStackTrace();
		} finally {
			factory.close();
		}

	}

}
```

Now run that. <br/>
Nice :) <br/>


**Hibernate and Primary Keys**
A primary key is a DBMS concept, basically it's a unique field that identifies each entry in a DB table.

***Note:*** I won't cover SQL or DBMS concepts here.

- @Id in Hibernate makes the annotated field a primary key.
- We can explicitly specify how Hibernate generates the Primary Key or @Id.
- @GeneratedValue(strategy=GenerationType.IDENTITY)
    This means that the primary key column will be used to generate primary key i.e. Auto-incremenet.
- Other strategies are:
    - GenerationType.AUTO: Hibernate will choose.
    - GenerationType.IDENTITY: the Identity col is the primary key. (most common)
    - GenerationType.SEQUENCE: a certain sequence is used.
    - GenerationType.TABLE: use another DB table to create primary keys.
- Check out Hibernate docs and your DB's docs for more info.
- We can make a custom generation strategy, how?
    - Implement interface **org.hibernate.id.IdentifierGenerator**.
    - Override the method **public Serializable generate(..)**.
    - **BE CAREFUL**
        1. It MUST create unique values.
        2. It's thread-safe.
        3. If it'll work on a cluster of servers, it will STILL generate unique values.

**Let's do some programming**
- Log into the MySQL workbench, right-click on your table, choose Alter Table.
- We won't really alter it, but we'll look at its structure.
    - PK: Primary key.
    - NN: Not null.
    - AI: Auto-increment.
    - etc.
- Now go to Eclipse.
- Student class.
- Add to the id field this annotation:
    @GeneratedValue(strategy=GenerationType.IDENTITY)
``` Java
	@Id
	@GeneratedValue(strategy = GenerationType.IDENTITY)
	@Column(name="id")
	private int id;
```

Then:
- Create a new class in com.luv2code.hibernate.demo package, called PrimaryKeyDemo.
- Copy the main function from CreateStudentDemo to PrimaryKeyDemo.
- Instead of one student, make many objects and commit them.
``` Java

public class PrimaryKeyDemo {
	public static void main(String[] args) {
		SessionFactory factory = new Configuration()
				.configure("hibernate.cfg.xml")
				.addAnnotatedClass(Student.class)
				.buildSessionFactory();
		
		Session session = factory.getCurrentSession();
		
		try {
			System.out.println("Creating student object...");
			Student s1 = new Student("Moamen", "Moataz", "ko2keeen@largestep.com");
			Student s2 = new Student("Atta", "Mostafa", "attaaa@largestep.com");
			Student s3 = new Student("Salaaaaah", "Thalaaaah", "salo7tche@largestep.com");
			
			System.out.println("Starting transaction...");
			session.beginTransaction();
			
			System.out.println("Saving the objects...");
			session.save(s1);
			session.save(s2);
			session.save(s3);
			
			System.out.println("Commit the transaction...");
			session.getTransaction().commit();
			
			System.out.println("Done.");
		} catch(Exception e) {
			e.printStackTrace();
		} finally {
			factory.close();
		}
	}
}
```

**How to change the starting index**
- In the workbench, open a new SQL tab and execute this SQL query:
``` sql
ALTER TABLE hb_student_tracker.student AUTO_INCREMENT=1000;
```
- Now all the new entries in the DB will start at 1000.
- Back to Eclipse, Re-run the code in PrimaryKeyDemo.
- Back to the workbench, check the table (by doing the query 
``` sql
SELECT * FROM hb_student_tracker.student;
```
).
- Check the result :)

**Empty the table**
- In the workbench, execute:
``` sql
truncate hb_student_tracker.student;
```

### Reading Objects with Hibernate
We retrieve objects from the DB using the primary key.
***Note:*** For any update or read, you ALWAYS use transactions.

**In code** <br/>
- In eclipse, copy the CreateStudentDemo class, paste it in the same package, but rename it as ReadStudentDemo.
- Open that new class.
- The code was copied and will include creating a Student and saving it.
- After the .commit() statement, get the primary key of the saved object by writing objectName.getId(). (Hibernate automatically updates a value when you save sth to the database, that value is returned by this function).
- Now, start another session using the SessionFactory(return it in the old session variable or a new one if you want).
- Begin a new transaction using the new session.
- Retrieve the student object using session.get(), passing in the student *Class* instance (Student.class) and the primary key of the object.
- If found, it'll be returned, else, null is returned.
- Commit the transaction.
``` java
public class PrimaryKeyDemo {
	public static void main(String[] args) {
		SessionFactory factory = new Configuration()
				.configure("hibernate.cfg.xml")
				.addAnnotatedClass(Student.class)
				.buildSessionFactory();
		
		Session session = factory.getCurrentSession();
		
		try {
            // Old code of saving stuff to DB, but with Daffy Duck
			Student daffyDuck = new Student("Daffy", "Duck", "daffy.duck@gmail.com");
			session.beginTransaction();
			session.save(daffyDuck);

			session.getTransaction().commit();
			
            // Getting the primary key
			System.out.println("Primary Key of daffy duck is: " + daffyDuck.getId());
			
            // new session
			session = factory.getCurrentSession();
			
            // new transaction
			session.beginTransaction();

            // reading from DB
			Student s = session.get(Student.class, daffyDuck.getId());
		
            // committing the transaction
			session.getTransaction().commit();
		} catch(Exception e) {
			e.printStackTrace();
		} finally {
			factory.close();
		}
	}
}

```

### Hibernate Query Language HQL
- A query langauge for retrieving objects in Hibernate.
- Very similar to SQL.
- For example, getting all students from the table:
``` java
List<Student> theStudents = 
            session.createQuery("from Student")
            .getResultList();
```
- getting all students where the last name is "Max":
``` Java
List<Student> theStudents = 
            session.createQuery("from Student s where s.lastName='Max'")
            .getResultList();
```
- getting all students where last name is "Max" or "Payne".
``` Java
List<Student> theStudents = 
            session.createQuery("from Student s where s.lastName='Max'" + " OR s.lastName='Payne'")
            .getResultList();
```
- getting the list of all students whose last name is LIKE "Max" with one character before it, like "CMax" or something.
``` Java
List<Student> theStudents = 
            session.createQuery("from Student s where s.lastName LIKE '%Max'"
            .getResultList();
```

As you can see, it's very similar toSQL.

**Querying With Hibernate**
- Copy and paste CreateStudentDemo class again, call the new one QueryStudentDemo.
- Query all students, then display them.
``` Java
public class QueryStudentDemo {

	public static void main(String[] args) {
		SessionFactory factory = new Configuration()
				.configure("hibernate.cfg.xml")
				.addAnnotatedClass(Student.class)
				.buildSessionFactory();
		
		Session session = factory.getCurrentSession();
		
		try {
			session.beginTransaction();
			
			List<Student> listOfStudents = session.createQuery("from Student").getResultList();
			
			for(Student student: listOfStudents) {
				System.out.println(student.toString());
			}
			
			session.getTransaction().commit();
		} catch(Exception e) {
			e.printStackTrace();
		} finally {
			factory.close();
		}
	}

}
```
- Try specifying a last name, adding OR, adding LIKE, etc.
***Note:*** Right-click on the for-loop that prints the students, choose Refactor -> Extract Method, give a Method name, click OK.
Eclipse will extract that loop, put it in a method, and replace it with the method in the main function.
**This is awesome :'D**
``` Java
public class QueryStudentDemo {

	public static void main(String[] args) {
		SessionFactory factory = new Configuration()
				.configure("hibernate.cfg.xml")
				.addAnnotatedClass(Student.class)
				.buildSessionFactory();
		
		Session session = factory.getCurrentSession();
		
		try {
			session.beginTransaction();
			
			List<Student> listOfStudents = session.createQuery("from Student").getResultList();
			
			printList(listOfStudents);
			
			session.getTransaction().commit();
		} catch(Exception e) {
			e.printStackTrace();
		} finally {
			factory.close();
		}
	}

	private static void printList(List<Student> listOfStudents) {
		for(Student student: listOfStudents) {
			System.out.println(student.toString());
		}
	}

}

```

### Updating Objects using Hibernate
- To update one object:
    1. we retrieve an object,
    2. we call the appropriate setters, 
    3. then commit the transaction.
 - To update a list of objects:
    1. we create an UPDATE query that gets that list and updates it,
    2. then we call .executeUpdate();

**Let's see this in code**
- Copy ReadStudentDemo class and create a new one "UpdateStudentDemo".
- Remove creating and reading code lines.
- Make an int variable that will hold a primary key that exists in the DB.
- Get the object with the primary key.
- Call setters on it.
- Commit the transaction.
``` Java
public class UpdateStudentDemo {

	public static void main(String[] args) {
		SessionFactory factory = new Configuration()
				.configure("hibernate.cfg.xml")
				.addAnnotatedClass(Student.class)
				.buildSessionFactory();
		
		Session session = factory.getCurrentSession();
		
		try {
			int studentId = 1;
			session.beginTransaction();
			
			// we get the object
			Student myStudent = session.get(Student.class, studentId);
			
			// we call setters to update it, 
			// because it's a persistent object so it can be updated this way
			myStudent.setFirstName("Manuel");
			myStudent.setLastName("Calavera");
			myStudent.setEmail("manny.calavera@dod.com");

			// Here it's actually committed in the database
			session.getTransaction().commit();
		} catch(Exception e) {
			e.printStackTrace();
		} finally {
			factory.close();
		}
	}

}
```

Now run it, then check the Workbench, see the table. <br/>
Real niiiiice :) <br/>

Let's try updating many students
- Use the session to create an update query to update records whose id is greater than 1.
- Execute the update.
- Commit the transaction.
``` java
		try {
			// previous work
			
			session = factory.getCurrentSession();
			session.beginTransaction();
			
            // Here's an update query with a where clause
			session.createQuery(
					"update Student"
					+ " set lastName='Delirious'"
					+ " where id > 1")
				.executeUpdate();
			
			session.getTransaction().commit();
			
		} catch(Exception e) {
			e.printStackTrace();
		} finally {
			factory.close();
		}
```
Run that piece of code, check that piece of workbench, refresh that piece of table.
Nice that piece of work, yo! :) <br\>

### Deleting Objects with Hibernate
- Get the object using Primary key.
- session.delete(the object)
- Remember that any change in the DB never actually occurs in the DB unless we COMMIT, so:
- Commit the transaction.

Alternate way:
- Create a delete query like
``` sql
DELETE FROM Student WHERE id=2;
```
- Execute the update.
- Commit.

***Code***
- Same procedure, copy UpdateStudentDemo to a new class DeleteStudentDemo.
- Remove all update lines.
- Get student with id = 3.
- Delete them.
- Commit the transaction.
- Create query to delete student with id = 2.
- Commit.
``` Java
public class DeleteStudentDemo {

	public static void main(String[] args) {
		SessionFactory factory = new Configuration()
				.configure("hibernate.cfg.xml")
				.addAnnotatedClass(Student.class)
				.buildSessionFactory();
		
		Session session = factory.getCurrentSession();
		
		try {
			int studentId = 3;
			session.beginTransaction();
			
			Student myStudent = session.get(Student.class, studentId);
			session.delete(myStudent);
			
			session.getTransaction().commit();
			
			session = factory.getCurrentSession();
			session.beginTransaction();
			
			session.createQuery(
					"delete Student"
					+ " where id = 2")
				.executeUpdate();
			
			session.getTransaction().commit();
			
		} catch(Exception e) {
			e.printStackTrace();
		} finally {
			factory.close();
		}
	}

}
```
Run that code, check the workbench, see the table, thank me later :P <br\>

**Now, we've covered how to do the basic CRUD applications with Hibernate**

## Hibernate Advanced Mappings
We will most likely have many tables and relationships between them.
And we'll need to model that in Hibernate, like:
- 1-to-1.
- 1-to-N, N-to-1.
- N-to-M.

### One-to-one mapping in Hibernate
For example:
- An instructor in an Instructor table, will have an instructor profile in another table.
- This is a 1-to-1 unidirectional relationship.

The instructorDetail table:
- id: int, primary key, auto increment.
- youtube_channel: varchar
- hobby: varchar

The instructor table:
- id: int, primary key, auto increment.
- first_name.
- last_name.
- email.
- instructor_detail_id: foreign key pointing to instructorDetail table.


#### Entity Lifecycle
This is a concept in Hibernate, it's basically a set of states where a Hibernate entity goes through:
- Detatched: entity is not linked to session.
- Merge: if detatched then entity will be linked to session.
- Persist: take new instance and save it to database on next flush/commit.
- Remove: remove entity from DB on next flush/commit.
- Refresh: sync data with DB.

#### One-to-one in Code
Steps:
1. Define database tables and setting foreign key.
2. Create InstructorDetail class.
3. Create Instructor class.
4. Create Main App.

**Step 1: Define database tables and setting foreign key**
In the workbench, execute these two queries:
``` sql
CREATE TABLE instructor_detail (
    id int(11) NOT NULL AUTO_INCREMENT,
	youtube_channel varchar(128) DEFAULT NULL,
    hobby varchar(45) DEFAULT NULL,
    PRIMARY KEY(id)
);

CREATE TABLE instructor (
    id int(11) NOT NULL AUTO_INCREMENT,
    first_name varchar(45) DEFAULT NULL,
    last_name varchar(45) DEFAULT NULL,
    email varchar(45) DEFAULT NULL,
    instructor_detail_id int(45) DEFAULT NULL,
    PRIMARY KEY(id),
    CONSTRAINT FK_DETAIL FOREIGN KEY (instructor_detail_id) REFERENCES instructor_detail(id)
);
```

**Step 2: Create InstructorDetail class**
- Basically create this class, name it, map the fields, generate constructors, getters, and setters, put it in com.luv2code.hibernate.demo.entity package for example.
``` Java
package com.luv2code.hibernate.demo.entity;

import javax.persistence.Column;
import javax.persistence.Entity;
import javax.persistence.Id;
import javax.persistence.Table;

@Entity
@Table(name="instructor_detail")
public class InstructorDetail {
	
	@Id
	@Column(name="id")
	private int id;
	
	@Column(name="youtube_channel")
	private String youtubeChannel;
	
	@Column(name="hobby")
	private String hobby;
	
	public InstructorDetail() {
		
	}

	public InstructorDetail(int id, String youtubeChannel, String hobby) {
		super();
		this.id = id;
		this.youtubeChannel = youtubeChannel;
		this.hobby = hobby;
	}

	public int getId() {
		return id;
	}

	public void setId(int id) {
		this.id = id;
	}

	public String getYoutubeChannel() {
		return youtubeChannel;
	}

	public void setYoutubeChannel(String youtubeChannel) {
		this.youtubeChannel = youtubeChannel;
	}

	public String getHobby() {
		return hobby;
	}

	public void setHobby(String hobby) {
		this.hobby = hobby;
	}

}

```

**Step 3: Create Instructor class**
- Same procedure, with some additional stuff:
- First, add an InstructorDetail field.
- Annotate the InstructorDetail field with @OneToOne.
- Then, annotate it again with @JoinColumn, passing the name of the column of the foreign key.
- That way, When Hibernate loads, it joins them together, and when it saves, it saves in separate tables.
``` Java
@Entity
@Table(name="instructor")
public class Instructor {
	
	@Id
	@Column(name="id")
	private int id;

	@Column(name="first_name")
	private String firstName;
	
	@Column(name="last_name")
	private String lastName;
	
	@Column(name="email")
	private String email;
	
	@OneToOne
	@JoinColumn(name="instructor_detail_id")
	private InstructorDetail instructorDetail;
	
	public Instructor() {
		
	}

	public Instructor(int id, String firstName, String lastName, String email, InstructorDetail instructorDetail) {
		super();
		this.id = id;
		this.firstName = firstName;
		this.lastName = lastName;
		this.email = email;
		this.instructorDetail = instructorDetail;
	}

	public int getId() {
		return id;
	}

	public void setId(int id) {
		this.id = id;
	}

	public String getFirstName() {
		return firstName;
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

	public InstructorDetail getInstructorDetail() {
		return instructorDetail;
	}

	public void setInstructorDetail(InstructorDetail instructorDetail) {
		this.instructorDetail = instructorDetail;
	}
	
}
```

**Step 4: Create Main App**
Cascading operations: perform an operation on an entity AND entities that are linked to it.
For example, if we save an instructor, we'll save its detail too, same with delete.
We can specify which operations to be cascaded through cascade Types:
- Persist: if entity is saved, related will be saved.
- Remove: if entity is deleted, related will be deleted.
- Refresh.
- Detatch.
- Merge.
- All: All of the above.

We pass the cascade type (or types in an array {t1, t2, t3 ...}) to the @OneToOne annotation.
***Note:*** By default, operations are **NOT cascaded**, if you don't specify cascading, there will be NO cascading.

Before the main app:
- Set cascade type to All in the instructor for the InstructorDetail field.

Creating the main app:
- Make an new main class called "CreateDemo".
- Make an Instructor object and an InstructorDetail object.
- set the Instructor object's detail to InstructorDetail.
- Begin a transaction.
- Save the instructor object.
- Commit the transaction.
``` Java
public class CreateDemo {

	public static void main(String[] args) {
		SessionFactory factory = new Configuration()
				.configure("hibernate.cfg.xml")
				.addAnnotatedClass(InstructorDetail.class)
				.addAnnotatedClass(Instructor.class)
				.buildSessionFactory();
		
		Session session = factory.getCurrentSession();
		
		try {
			InstructorDetail detail = new InstructorDetail("yt.com", "football");
			Instructor instructor = new Instructor("Max", "Payne", "i@a.com");
			instructor.setInstructorDetail(detail);
			
			session.beginTransaction();
			
			session.save(instructor);

			session.getTransaction().commit();
		} catch(Exception e) {
			e.printStackTrace();
		} finally {
			factory.close();
		}
	}

}
```

Now run that, see the workbench, and celebrate :) <br/>

**What about deleting an instructor?**
Try deleting an instructor in the main app and check the workbench.

#### Bi-Directional One-to-one
- If we have an InstructorDetail, we'd also like to get the associated instructor.
- But we can't do that with our current relationship.
- So, we need to MAP the Instructor to the InstructorDetail i.e. InstructorDetail also references Instructor.

Because Hibernate is awesome:
- We don't need to change the database schema, which is good.
- We just need to update the Java code.

Steps:
1. Update the InstructorDetail class by:
	+ adding a new field to reference Instructor,
	+ adding getters/setters for it,
	+ adding @OneToOne annotation, but pass a new parameter 'mappedBy="instructorDetail"'
2. Create the Main App.

**What is mappedBy?**
- It's an argument passed to @OneToOne.
- It tells Hibernate "look at the instructorDetail property in the Instructor class".
- Hibernate can use the information from instructorDetail to know how these two items are linked.
- Which it can use to get the Instructor class referencing it.
- Doing that, we have a bi-directional relationship WITHOUT using a new foreign key in the InstructorDetail table (i.e. without changing the DB).
- Which is good.

**Which class gets the mappedBy argument?**
- Two classes mapped to two tables.
- One of them has a foreign key referencing the other table.
- The OTHER class gets the mappedBy, and doesn't get @JoinColumn.

**Step 1: Update the InstructorDetail class**
- Add an instructor field.
- generate getters & setters.
- Add @OneToOne to the new field, but pass a new parameter 'mappedBy="instructorDetail"'.
- Also, add a cascade argument and pass CascadeType.ALL to it.
``` Java
import javax.persistence.CascadeType;
import javax.persistence.Column;
import javax.persistence.Entity;
import javax.persistence.GeneratedValue;
import javax.persistence.GenerationType;
import javax.persistence.Id;
import javax.persistence.OneToOne;
import javax.persistence.Table;

@Entity
@Table(name="instructor_detail")
public class InstructorDetail {
	
	@Id
	@GeneratedValue(strategy=GenerationType.IDENTITY)
	@Column(name="id")
	private int id;
	
	@Column(name="youtube_channel")
	private String youtubeChannel;
	
	@Column(name="hobby")
	private String hobby;
	
	@OneToOne(mappedBy="instructorDetail", cascade=CascadeType.ALL)
	private Instructor instructor;
	
	public InstructorDetail() {
		
	}

	public InstructorDetail(String youtubeChannel, String hobby) {
		this.youtubeChannel = youtubeChannel;
		this.hobby = hobby;
	}
	
	public int getId() {
		return id;
	}

	public void setId(int id) {
		this.id = id;
	}

	public String getYoutubeChannel() {
		return youtubeChannel;
	}

	public void setYoutubeChannel(String youtubeChannel) {
		this.youtubeChannel = youtubeChannel;
	}

	public Instructor getInstructor() {
		return instructor;
	}

	public void setInstructor(Instructor instructor) {
		this.instructor = instructor;
	}

	public String getHobby() {
		return hobby;
	}

	public void setHobby(String hobby) {
		this.hobby = hobby;
	}

	@Override
	public String toString() {
		return "InstructorDetail [id=" + id + ", youtubeChannel=" + youtubeChannel + ", hobby=" + hobby + "]";
	}
}
```

**Step 2: Create the main app**
- Read an instructorDetail from the DB.
- Get the instructor associated with the instructorDetail you read from the DB and print it.
``` Java
public class CreateBiDemo {

	public static void main(String[] args) {
		SessionFactory factory = new Configuration()
				.configure("hibernate.cfg.xml")
				.addAnnotatedClass(InstructorDetail.class)
				.addAnnotatedClass(Instructor.class)
				.buildSessionFactory();
		
		Session session = factory.getCurrentSession();
		
		try {
			int theId = 2;
			
			session.beginTransaction();
			
			InstructorDetail detail = session.get(InstructorDetail.class, theId);
			
			System.out.println(
					"The detail " + detail.toString() + 
					" is associated to instructor: " + detail.getInstructor());
			
			session.getTransaction().commit();
		} catch(Exception e) {
			e.printStackTrace();
		} finally {
			factory.close();
		}
	}

}
```
Run that awesomeness, you'll get awesomeness :)

#### Some Refactoring and Exception Handling
- If theId didn't exist in DB, reading will return null, so when we print that, a NullPointerException will be raised.
- Also, another exception will be raised, a Leak exception, because the session wasn't closed when the NullPointerException occured.
- So, let's add a Catch block (I already did earlier, in the course he just added it).
- Also, in the finally block, close the session:
``` Java
public class CreateBiDemo {

	public static void main(String[] args) {
		SessionFactory factory = new Configuration()
				.configure("hibernate.cfg.xml")
				.addAnnotatedClass(InstructorDetail.class)
				.addAnnotatedClass(Instructor.class)
				.buildSessionFactory();
		
		Session session = factory.getCurrentSession();
		
		try {
			int theId = 2;
			
			session.beginTransaction();
			
			InstructorDetail detail = session.get(InstructorDetail.class, theId);
			
			System.out.println(
					"The detail " + detail.toString() + 
					" is associated to instructor: " + detail.getInstructor());
			
			session.getTransaction().commit();
		} catch(Exception e) {
			e.printStackTrace();
		} finally {
			session.close();
			factory.close();
		}
	}

}
```

**Deleting an InstructorDetail**
- Try deleting an InstructorDetail object from the DB, since the Instructor is also cascaded (because it's bi-directional), the associated instructor should be deleted too.
``` Java
public class DeleteInstructorDetailDemo {

	public static void main(String[] args) {
		SessionFactory factory = new Configuration()
				.configure("hibernate.cfg.xml")
				.addAnnotatedClass(InstructorDetail.class)
				.addAnnotatedClass(Instructor.class)
				.buildSessionFactory();
		
		Session session = factory.getCurrentSession();
		
		try {
			int theId = 3;
			
			session.beginTransaction();
			
			InstructorDetail detail = session.get(InstructorDetail.class, theId);
			
			session.delete(detail);
			
			session.getTransaction().commit();
		} catch(Exception e) {
			e.printStackTrace();
		} finally {
			session.close();
			factory.close();
		}
	}

}
```

Run that, look at the workbench, both tables will have one item deleted, because:
- We.
- Are.
- Awesome.
B| :) <br/> 

**What if we want to delete the InstructorDetail?**
- Change the CascadeType in InstructorDetail to all of them in curly braces except for REMOVE.
``` Java
public class InstructorDetail {
	...
	
	@OneToOne(mappedBy="instructorDetail", 
			cascade={CascadeType.DETACH,
					CascadeType.MERGE,
					CascadeType.PERSIST,
					CascadeType.REFRESH})
	private Instructor instructor;
	
	...
}
```
Now, back to the Main App, change the id to something that's in the DB, and run it. <br/>

You thought it would run?
You did :P
No, an exception will be thrown that says something "deleted object would be re-saved by cascade".

**What is that?**
- Basically, we need to break the bi-directional link, because some instructor will be pointing to the instructorDetail we're trying to delete, we need to break THAT LINK.
- To solve that, before we delete the instructorDetail, we get the associated instructor, set its detail to null, THEN delete the detail.
``` Java
			session.beginTransaction();
			
			InstructorDetail detail = session.get(InstructorDetail.class, theId);
			
			// This is the new line we added
			detail.getInstructor().setInstructorDetail(null);
			
			session.delete(detail);
			
			session.getTransaction().commit();
```
Now run the main app again.
Then go to the workbench, check the instructorDetail table, an entry will have been deleted.
Then, check the Instructor table, no entry will be deleted, but there will be an instructor that has instructor_detail_id set to null.

*This is how we do it. [[dancing]]*

### One-to-many mapping in Hibernate
- Instructor -> many courses.
Real use-case requirements may be:
- If we delete an instructor, **do not** delete the courses, and vv.
i.e. don't apply cascading delete.

Steps:
1. Define Database tables.
2. Create Course class.
3. Update Instructor class.
4. Add some convenient methods to Instructor e.g. addCourse.
4. Create main app.

Course table will have:
- id. (Primary Key)
- title. (Unique Key)
- instructor_id. (Foreign key that references instructor table)

The Instructor will have a 1-to-many relationship with course (one instructor can give many courses).
The course will have many-to-1 relationship with instructor (Many courses can have one instructor).

**Step 1: Define database tables**
- Execute this script in the workbench:
``` sql
DROP SCHEMA IF EXISTS `hb-03-one-to-many`;

CREATE SCHEMA `hb-03-one-to-many`;

use `hb-03-one-to-many`;

SET FOREIGN_KEY_CHECKS = 0;

DROP TABLE IF EXISTS `instructor_detail`;

CREATE TABLE `instructor_detail` (
  `id` int(11) NOT NULL AUTO_INCREMENT,
  `youtube_channel` varchar(128) DEFAULT NULL,
  `hobby` varchar(45) DEFAULT NULL,
  PRIMARY KEY (`id`)
) ENGINE=InnoDB AUTO_INCREMENT=1 DEFAULT CHARSET=latin1;


DROP TABLE IF EXISTS `instructor`;

CREATE TABLE `instructor` (
  `id` int(11) NOT NULL AUTO_INCREMENT,
  `first_name` varchar(45) DEFAULT NULL,
  `last_name` varchar(45) DEFAULT NULL,
  `email` varchar(45) DEFAULT NULL,
  `instructor_detail_id` int(11) DEFAULT NULL,
  PRIMARY KEY (`id`),
  KEY `FK_DETAIL_idx` (`instructor_detail_id`),
  CONSTRAINT `FK_DETAIL` FOREIGN KEY (`instructor_detail_id`) 
  REFERENCES `instructor_detail` (`id`) ON DELETE NO ACTION ON UPDATE NO ACTION
) ENGINE=InnoDB AUTO_INCREMENT=1 DEFAULT CHARSET=latin1;

DROP TABLE IF EXISTS `course`;

CREATE TABLE `course` (
  `id` int(11) NOT NULL AUTO_INCREMENT,
  `title` varchar(128) DEFAULT NULL,
  `instructor_id` int(11) DEFAULT NULL,
  
  PRIMARY KEY (`id`),
  
  UNIQUE KEY `TITLE_UNIQUE` (`title`),
  
  KEY `FK_INSTRUCTOR_idx` (`instructor_id`),
  
  CONSTRAINT `FK_INSTRUCTOR` 
  FOREIGN KEY (`instructor_id`) 
  REFERENCES `instructor` (`id`) 
  
  ON DELETE NO ACTION ON UPDATE NO ACTION
) ENGINE=InnoDB AUTO_INCREMENT=10 DEFAULT CHARSET=latin1;


SET FOREIGN_KEY_CHECKS = 1;
```
Make sure to fix the TestJDBC and the cfg file for the new schema "hb-03-one-to-many".

**Step 2: Create Course class**
- Create the class and add the fields id, title, instructor.
- Annotate the instructor with @ManyToOne, cascade all BUT the delete, also @JoinColumn.
- Generate getters, setters, constructor, etc.
``` Java

public class Course {
	
	@Id
	@GeneratedValue(strategy=GenerationType.IDENTITY)
	@Column(name="id")
	private int id;
	
	@Column(name="title")
	private String title;
	
	@ManyToOne(cascade={CascadeType.MERGE,
			CascadeType.PERSIST,
			CascadeType.REFRESH,
			CascadeType.DETACH})
	@JoinColumn(name="instructor_id")
	private Instructor instructor;
	
	public Course() {
		
	}
	

	public Course(String title) {
		this.title = title;
	}


	public int getId() {
		return id;
	}

	public void setId(int id) {
		this.id = id;
	}

	public String getTitle() {
		return title;
	}

	public void setTitle(String title) {
		this.title = title;
	}

	public Instructor getInstructor() {
		return instructor;
	}

	public void setInstructor(Instructor instructor) {
		this.instructor = instructor;
	}


	@Override
	public String toString() {
		return "Course [id=" + id + ", title=" + title + "]";
	}
}
```

**Step 3 and 4: Update Instructor class and add some methods for convenience**
- Add a new List<Course> field.
- Annotate this with @oneToMany, pass it the mappedBy="instructor" argument.
- Add the cascading for all WITHOUT the delete.
- Generate getters and setters for the new field.
- Add a method addCourse, which adds a Course to the Instructors's list, AND sets the new course's instructor to the caller object.
``` Java

@Entity
@Table(name="instructor")
public class Instructor {
	...
	
	@OneToMany(mappedBy="instructor",
			cascade={CascadeType.MERGE,
			CascadeType.PERSIST,
			CascadeType.REFRESH,
			CascadeType.DETACH})
	private List<Course> courses;

	...

	public List<Course> getCourses() {
		return courses;
	}

	public void setCourses(List<Course> courses) {
		this.courses = courses;
	}

	public void addCourse(Course c) {
		if(courses == null) {
			courses = new ArrayList<Course>();
		}
		courses.add(c);
		c.setInstructor(this); // this is to set the foreign key of the new course
	}

	@Override
	public String toString() {
		return "Instructor [id=" + id + ", firstName=" + firstName + ", lastName=" + lastName + ", email=" + email + "]";
	}
	
}
```

**Step 4: Create Main App**
We'll create FOUR apps:
1. CreateInstructorDemo: this adds some instructors to the DB.
Run this one, then go to the next one.
``` Java
public class CreateInstructorDemo {

	public static void main(String[] args) {

		// create session factory
		SessionFactory factory = new Configuration()
								.configure("hibernate.cfg.xml")
								.addAnnotatedClass(Instructor.class)
								.addAnnotatedClass(InstructorDetail.class)
								.addAnnotatedClass(Course.class)
								.buildSessionFactory();
		
		// create session
		Session session = factory.getCurrentSession();
		
		try {			
			
			// create the objects			
			Instructor tempInstructor = 
					new Instructor("Susan", "Public", "susan.public@luv2code.com");
			
			InstructorDetail tempInstructorDetail =
					new InstructorDetail(
							"http://www.youtube.com",
							"Video Games");		
			
			// associate the objects
			tempInstructor.setInstructorDetail(tempInstructorDetail);
			
			// start a transaction
			session.beginTransaction();
			
			// save the instructor
			//
			// Note: this will ALSO save the details object
			// because of CascadeType.ALL
			//
			System.out.println("Saving instructor: " + tempInstructor);
			session.save(tempInstructor);					
			
			// commit transaction
			session.getTransaction().commit();
			
			System.out.println("Done!");
		}
		finally {
			
			// add clean up code
			session.close();
			
			factory.close();
		}
	}

}
```
2. CreateCourseDemo: this adds some courses to the DB and associates them with instructors from the DB.
Run that, then move on.
``` Java
public class CreateCoursesDemo {

	public static void main(String[] args) {

		// create session factory
		SessionFactory factory = new Configuration()
								.configure("hibernate.cfg.xml")
								.addAnnotatedClass(Instructor.class)
								.addAnnotatedClass(InstructorDetail.class)
								.addAnnotatedClass(Course.class)
								.buildSessionFactory();
		
		// create session
		Session session = factory.getCurrentSession();
		
		try {			
			Course c1 = new Course("pm");
			Course c2 = new Course("athar be2y");
			Course c3 = new Course("data communication");
			Course c4 = new Course("measurements");
			Course c5 = new Course("handasa madanya");
			
			session.beginTransaction();
			
			Instructor i1 = session.get(Instructor.class, 1);
			Instructor i2 = session.get(Instructor.class, 2);
			
			c1.setInstructor(i1);
			c2.setInstructor(i1);
			c3.setInstructor(i1);
			c4.setInstructor(i2);
			c5.setInstructor(i2);
			
			session.save(c1);
			session.save(c2);
			session.save(c3);
			session.save(c4);
			session.save(c5);
			
			session.getTransaction().commit();
		}
		finally {
			
			// add clean up code
			session.close();
			
			factory.close();
		}
	}

}
```
3. GetInstructorCoursesDemo: this gets an instructor and displays its list of courses.
Run that.
``` Java
public class GetInstructorCoursesDemo {

	public static void main(String[] args) {

		// create session factory
		SessionFactory factory = new Configuration()
								.configure("hibernate.cfg.xml")
								.addAnnotatedClass(Instructor.class)
								.addAnnotatedClass(InstructorDetail.class)
								.addAnnotatedClass(Course.class)
								.buildSessionFactory();
		
		// create session
		Session session = factory.getCurrentSession();
		
		try {
			session.beginTransaction();
			
			Instructor i1 = session.get(Instructor.class, 1);
			
			System.out.println("Courses for i1: " + i1.getCourses());
				
			session.getTransaction().commit();
		}
		finally {
			
			// add clean up code
			session.close();
			
			factory.close();
		}
	}

}
```
4. DeleteCourseDemo: this delets a course and ensures that no instructor deletion happens, and takes care of any mapping links that should be broken.
``` Java

public class DeleteCourseDemo {

	public static void main(String[] args) {

		// create session factory
		SessionFactory factory = new Configuration()
								.configure("hibernate.cfg.xml")
								.addAnnotatedClass(Instructor.class)
								.addAnnotatedClass(InstructorDetail.class)
								.addAnnotatedClass(Course.class)
								.buildSessionFactory();
		
		// create session
		Session session = factory.getCurrentSession();
		
		try {
			session.beginTransaction();
			
			Course toBeDeleted = session.get(Course.class, 11);
			
			session.delete(toBeDeleted);
			
			session.getTransaction().commit();
		}
		finally {
			
			// add clean up code
			session.close();
			
			factory.close();
		}
	}

}
```
Run that, then check the DB in the workbench.
Also, run the GetInstructorCoursesDemo app for the instructor that had the deleted course, and see the result.

Awesome :) <br/>

