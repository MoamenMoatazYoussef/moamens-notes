# Hibernate
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



