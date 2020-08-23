# Installing Tomcat
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

# Installing Eclipse
1. Go to www.eclipse.org, choose Download, Download Packages, choose Eclipse For Java EE Developers, or Eclipse for Enterprise Java Developers, then click Download, and wait.
2. Extract the zip file anywhere, I put it at C:\eclipse
3. Inside it, you can actually start eclipse.
4. Select anywhere as your workspace, just a folder where all the source colde will be.
5. Check that when it opens, it's called Eclipse Java EE IDE for Web Developers, now you succeeded in installation.

# Connecting Tomcat to Eclipse
In eclipse:
1. close the welcome tab.
2. go to bottom section, choose Servers tab.
3. click on the link to add a server.
4. in the window that opens, expand the folder Apache, choose Tomcat server with the version you have, then 'next'.
5. Choose the tomcat installation directory as the place where you installed tomcat (obviously xD), default is C:\Program Files\Apache Software Foundation\Tomcat <version number>
Then click 'next'
6. Click 'finish'.