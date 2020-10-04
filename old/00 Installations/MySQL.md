# Setting up MySQL on your PC
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
