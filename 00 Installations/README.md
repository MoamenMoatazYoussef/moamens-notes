# All Installations
## Table of Contents
- [All Installations](#all-installations)
  * [Installing Maven on Windows](#installing-maven-on-windows)
  * [Setting up MySQL on your PC](#setting-up-mysql-on-your-pc)
  * [Installing Tomcat](#installing-tomcat)
  * [Eclipse and integration with other stuff](#eclipse-and-integration-with-other-stuff)
    + [Installing Eclipse](#installing-eclipse)
    + [Setting up Hubernate in Eclipse IDE](#setting-up-hubernate-in-eclipse-ide)
    + [Installing Maven Plugin in Eclipse](#installing-maven-plugin-in-eclipse)

<small><i><a href='http://ecotrust-canada.github.io/markdown-toc/'>Table of contents generated with markdown-toc</a></i></small>

## Installing Maven on Windows
Maven is weird to install because we don't really "install" it, we download an already compiled version for our respective platform, then make it available on the java classpath.
- Go to this link:
https://maven.apache.org/download.cgi
- Go to the Files section, download the version that's compatible for your environment, for windows it's the one ending with "bin.zip".
- It's an 8 MB zip file so it won't take long.
- Extract it, and copy the folder to a path you like, the path used in the course is: "C:/Users/<your user name>/mvn", but I'll use "D:/dev_stuff".
- Now, open the start menu and search for env, open "Edit the system Environment Variables".
- Click "Environment Variables..."
- Add the JAVA_HOME system variable:
    + In the lower section, click New, give a variable name of "JAVA_HOME", 
    + give the path of the JDK main folder, for example C:/Program Files/Java/jdk-version", select this, don't select the bin folder.
- Then, go to the PATH variable in both sections (wherever you find it):
    + Add to it the path to the maven folder we extracted/bin, in the *bin* folder, not the external folder.
- Finally, close this and open a command prompt.
- Type mvn -v, see if it'll display the version, if it does then you've successfully set up maven.

## Setting up MySQL on your PC
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
- That's it, you verified the installation :smile:

***Note:*** Official MySQL installation instructions:
https://dev.mysql.com/doc/refman/5.7/en/osx-installation.html


## Installing Tomcat
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

## Eclipse and integration with other stuff
### Installing Eclipse
1. Go to www.eclipse.org, choose Download, Download Packages, choose Eclipse For Java EE Developers, or Eclipse for Enterprise Java Developers, then click Download, and wait.
2. Extract the zip file anywhere, I put it at C:\eclipse
3. Inside it, you can actually start eclipse.
4. Select anywhere as your workspace, just a folder where all the source colde will be.
5. Check that when it opens, it's called Eclipse Java EE IDE for Web Developers, now you succeeded in installation.

### Setting up Hubernate in Eclipse IDE
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

### Installing Maven Plugin in Eclipse
- Open eclipse.
- Open the menu Help -> Install New Software... -> what is already installed? 
    - if the list contains m2e and m2e-wtp, we're good to go, skip the next sub-steps.
    - If not, go to the previous screen (install new software...).
    - Click Add...
    - In the Name, enter "m2e update site"
    - Check the box next to "Maven Integration for Eclipse".
    - Next, Next, Accept license agreement, Finish.
    - Restart Eclipse.
    - Check the "already installed" list for the m2e and m2e-wtp to verify installation.

