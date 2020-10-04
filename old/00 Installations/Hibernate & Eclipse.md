# Setting up Hubernate in Eclipse IDE
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