# Yarn

A package manager. Also uses *package.json* file.
install:
https://yarnpkg.com/en/docs/install#windows-stable

Basic commands:
``` sh
yarn init
yarn add [package]@[version]
yarn upgrade [package]
yarn remove [package]
```

Installing all dependencies:
```
yarn
```

## Workflow of Yarn
1. Creating a new project.
2. Adding/updating/removing deps.
3. Installing/reinstalling deps.
4. working with git.
5. Continuous integration.

### Creating a new project
Adding yarn to an existing repo/root directory of code, or starting a project from scratch, are the same for Yarn.

First, to add Yarn:
``` sh
yarn init
```
Then fill in the input prompts that will pop up in the CLI.

This will only create the *package.json* file, example of it:
``` json
{
  "name": "my-new-project",
  "version": "1.0.0",
  "description": "My New Project description.",
  "main": "index.js",
  "repository": {
    "url": "https://example.com/your-username/my-new-project",
    "type": "git"
  },
  "author": "Your Name <you@example.com>",
  "license": "MIT"
}
```
A list of its fields: https://yarnpkg.com/en/docs/package-json

Another file is the *yarn.lock* file, this file stores exactly which versions of each dependency were installed.

This file is handled entirely by yarn, so you shouldn't edit it at all. (If you do, you may break something, so don't)

This file should be included in your git, so that any other machine uses the project gets the same exact dependency tree and the same versions.

Example of it:
```
# THIS IS AN AUTOGENERATED FILE. DO NOT EDIT THIS FILE DIRECTLY.
# yarn lockfile v1
package-1@^1.0.0:
  version "1.0.3"
  resolved "https://registry.npmjs.org/package-1/-/package-1-1.0.3.tgz#a1b2c3d4e5f6g7h8i9j0k1l2m3n4o5p6q7r8s9t0"
package-2@^2.0.0:
  version "2.0.1"
  resolved "https://registry.npmjs.org/package-2/-/package-2-2.0.1.tgz#a1b2c3d4e5f6g7h8i9j0k1l2m3n4o5p6q7r8s9t0"
  dependencies:
    package-4 "^4.0.0"
package-3@^3.0.0:
  version "3.1.9"
  resolved "https://registry.npmjs.org/package-3/-/package-3-3.1.9.tgz#a1b2c3d4e5f6g7h8i9j0k1l2m3n4o5p6q7r8s9t0"
  dependencies:
    package-4 "^4.5.0"
package-4@^4.0.0, package-4@^4.5.0:
  version "4.6.3"
  resolved "https://registry.npmjs.org/package-4/-/package-4-2.6.3.tgz#a1b2c3d4e5f6g7h8i9j0k1l2m3n4o5p6q7r8s9t0"
```
### Managing dependencies
We can add, upgrade, or remove dependencies or packages.
When we do that, the file *package.json* and the file *yarn.lock* will both be changed.

Adding a package:
``` sh
yarn add [package]
```
This will add this entry in the *package.json* and update the *yarn.lock* to reflect that change:
``` json
  {
    "name": "my-package",
    "dependencies": {
        "package-1": "^1.0.0"
    }
  }
```

Flags for this command:
--dev: for dev dependencies.
--peer: for peer dependencies.
--optional: for optional dependencies.
@[version]: to specify the version of the package to install.

Upgrading a package:
``` sh
yarn upgrade [package]
```

Removing a package:
``` sh
yarn remove [package]
```

Both updating and removing take the same flags as adding.

### Installind dependencies
Adding dependencies automatically install them, but if you got a project from git or whatever and the package.json file already has the dependencies, only they're not installed, you will need to install them:
``` sh
yarn
```
or 
``` sh
yarn install
```
These commands install all dependencies for the project.

Flags:
--flat: to install only one version of the package.
--force: forces re-downloading of all packages.
--production: install only production dependencies.
And others: https://yarnpkg.com/en/docs/cli/install

## More commands
- Publishing to yarn: yarn publish
- Running user-defined scripts: yarn [script] [args]
- Running user-defined commands: yarn [command] [args]
- Verbose output: yarn [command] --verbose
A lot more here:
https://yarnpkg.com/en/docs/cli/

Analogy with npm commands:
https://yarnpkg.com/en/docs/migrating-from-npm

### --mutex
when we have different yarn instances running, we can make sure that only one is running at a given time to ensure no conflicts by passing the global --mutex followed by *file* or *network*.

*file*: this will write/read a mutex file .yarn-single-instance in the current working directory, or you can give it an alternate name.
``` sh
--mutex file
--mutex file:/path/to/.yarn-mutex
```

*network*: you can specify a port for yarn instances to use, or the default is 31997.
``` sh
--mutex network
--mutex network:30330
```

## Migrating from npm
0. go to an npm project
1. install dependencies using yarn i.e. run the command: ***yarn***
2. add the *yarn.lock* file that was created to your git.
3. you can also import the *package-lock.json* using the command ***yarn import***

Others can keep using npm, don't worry about others needing to convert to yarn.

To go back to npm, you can just use npm with no additional steps, if you want, you can delete the *yarn.lock* file.


## Creating a package
1. Make a new git repo/directory for your package.
2. cd into it.
3. **yarn init**
``` sh
git init my-new-project
cd my-new-project
yarn init
```

A package.json file will be created inside the directory.

## Publishing a package
Yarn uses the npm registry to distribute packages globally.
1. Create an npm account.
2. Use this command:
``` sh
yarn login
```
3. Publish your package:
``` sh
yarn publish
```

Enter the info, and that's it.

## Accessing your package
Now your package will be on the npm registry i.e. you will find it at https://www.npmjs.com/package/my-new-project, you should be able to install it like any normal package:
``` sh
yarn add my-new-project
```

## Types of dependencies
- Dependencies: these are the normal ones.
- devDependencies: dependencies only needed in development.
- peerDependencies: dependencies that help when we publish our own package.
- optionalDependencies: if they fail to install, yarn will still say the install process was successful.
- bundledDependencies.

## Configuring your package
Through package.json and yarn.lock files, and also:
- .yarnrc files: these allow us to configure additional yarn features, check it here: https://yarnpkg.com/en/docs/yarnrc

## Workspaces