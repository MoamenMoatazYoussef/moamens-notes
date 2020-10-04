# Node package Manager (NPM)

Also known as:
Nano Pico Micro, Nana..Pat Man, Ne Pas Manger!, Never Play Metal, Never Post Memes, Never Push to Master, Nobody's Perfect, Man, No Pants Mafia...and many many others, go to https://www.npmjs.com/ and click a few times on the name on the top left xD

## Table of Contents

## Tips

## Intro
npm is the world'ts largest software registry. You use it to get software libraries and packages, share them, etc.

You can also have a private space where specific people can share packages and stuff.

It's basically Maven for frontend developers.

To start, Go to npmjs.com and create an account. Read more about account settings and two-factor auth here:
https://docs.npmjs.com/getting-started/setting-up-your-npm-user-account

Check out this whole secion here:
https://docs.npmjs.com/getting-started/

## Packages and Modules
The npm registry is a DB of JS packages, each consists of JS code and Meta data.

### Packages
A package is a file or directory that has a file inside it called *package.json*, this is a JSON file that contains the metadata and configurations of the package, like Version, Dependencies, Scripts, etc.

A package can be:
- a) A folder containing a program described by a package.json file.
- b) A gzipped tarball containing (a).
- c) A URL that resolves to (b).
- d) A <name>@<version> that is published on the registry with (c).
- e) A <name>@<tag> that points to (d).
- f) A <name> that has a latest tag satisfying (e).
- g) A git url that, when cloned, results in (a).

Git URLs used for npm packages can be formatted in the following ways:
git://github.com/user/project.git#commit-ish
git+ssh://user@hostname:project.git#commit-ish
git+http://user@hostname/project/blah.git#commit-ish
git+https://user@hostname/project/blah.git#commit-ish
The commit-ish can be any tag, sha, or branch that can be supplied as an argument to git checkout. The default commit-ish is master.

In any package, you'll find a folder called *node_modules*, this includes any Modules that the package needs (dependencies).

### Modules
A module is any file or directory in the *node_modules* folder.
Modules are loaded by the Node.js's function **require()**.

To make that possible, a module must be one of the following:
- A folder with package.json file containing a "main" field.
- A folder with index.js file in it.
- A JS file.

So, not all modules are packages.

### Create package.json
This file:
- Lists the dependencies you need for a certain project or package.
- Specifies versions, this is used to conform to semantic versioning rules enforced by npm, look here: https://docs.npmjs.com/about-semantic-versioning
- Makes your build reproducible, and also you don't have to include the dependencies with your package when uploading it, other developers can download your package and install the rest using package.json i.e. much less memory used.

Fields of the package.json file:
- name: your package's name, must be lowercase one word, may contain hyphens or underscores.
- version: in the form x.x.x and follows the semantic versioning rules mentioned above.

Others are:
- author: the format is: "your-name your-mail your-website"
- description
- main: this field's value always has to be index.js
- scripts
- keywords
- license
- bugs
- homepage

To create a package.json file in your project with values you supply:
``` sh
cd your/package/path
npm init
```
Answer the questions in the CLI questionnaire.

### Create a Node module
1. Create a package.json file.
2. Create the file that will be loaded when your module is required by another application.
The default name of this file is index.js.
In this file, add a function as a property of an object called *exports*, this object is the object that will be loaded into other apps. So any functions inside it will be available to use.
``` js
exports.doThis = function() {
    // write your code here
}
```
3. Publish it to npm:
For public:
``` sh
npm publish
```

For scoped public:
``` sh
npm publish --access public
```

4. cd outside your project directory and create another directory for testing.
5. cd into it.
6. install the module you just created:
``` sh
npm install <your module name>
```
7. Create a test.js file that *requires* your module and calls it as a method.
8. To test your module, run this command:
``` sh
node test.js
```

### Dependencies in package.json
There are two things:
*dependencies*: dependencies required in production.
*devDependencies*: dependencies required for local dev & testing only.

To add dependencies:
**Method 1: add into the JSON file manually** <br/>
The dependency entry looks like this:
``` json
// this is the whole package.json file
{
  "name": "my_package",
  "version": "1.0.0",
  "dependencies": {
      // Look here, just a simple line with name: version
    "my_dep": "^1.0.0",
    "another_dep": "~2.2.0"
  }
}
```

**Method 2: Using the CLI:**
Run this to add to dependencies:
``` sh
npm install <package-name> [--save-prod]
```
Or this to add to devDependencies:
``` sh
npm install <package-name> --save-dev
```

To install a package with a specified version, check here:
https://docs.npmjs.com/adding-dist-tags-to-packages

