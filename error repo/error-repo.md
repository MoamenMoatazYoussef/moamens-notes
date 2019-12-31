# Error Repo

## Frontend

#### useHistory hook not imported from react-router-dom
**Environment:** ReactJS, using hooks.
**Symptom:** Failed to compile: useHistory is not imported/exported from react-router-dom.
**Possible causes and solutions:**
- Case 1:
	- **Problem:** react-router-dom's version is below 5.1.2.
	- **Solution(s)**, do **ONLY ONE** of the following:
		- Change the version in package.json to 5.1.2, then delete node_modules, then npm install.
		- npm update.

## Backend

#### Docker hangs at mounting on database operation
**Environment:** Nodejs + AWS SAM + Docker + Postgres (Or any SQL Server)
**Symptom:** Docker hangs at "mounting image", produces no logs.
**Possible causes and solutions:**
- Case 1:
	- **Problem:** You're performing a database operation, and an error occurs in it, but you don't "await" the operation, so error is not reported back, and docker hangs.
	- **Solution(s):** "await" the operation, and add exception handling to respond with 400 or 500 accordingly.
	

