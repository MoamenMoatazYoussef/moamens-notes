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
