# Installing Maven on Windows
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