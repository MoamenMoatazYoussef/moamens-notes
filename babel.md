Babel:
======
Babel is a transpiler, it translates ES6 JS to ES5 and older JS so that browsers that don't support ES6 yet can run the app we're developing with JS. (e.g. arrow functions, spreading, etc.)

How does it work:
- Babel takes source code.
- Uses a parser called Babylon that converts the source code into an abstract syntax tree.
- Then it traverses the tree step by step and transforms each "whole step" into modified code (es5).
- Which is used to generate the output.

Plugins:
--------
Babel supports plugins, which are small JS programs that tell babel how to transpile the code.
Some already-made plugins and presets are made for Babel. e.g.
- @babel/preset-react: to transpile react's JSX to es5.
- @babel/preset-env: transpile the latest JS (whatever it is) to es5.
- @babel/preset-typescript: transpiles typescript to es5.
- @babel/polyfill: basically this is a polyfill that provides all non-es5 stuff.

So, babel consists of:
- Presets, which has 
- Plugins.
- Polyfills for es5 and stuff.

Configuration:
- Two files used to configure babel: "babel.config.js" and ".babelrc".
- For example, we can configure babel to target specific browsers, use specific plugins and give them properties (although it usually auto-installs the necessary plugins when you specify the target browsers).
- Check the docs for more config options.


Getting started with babel:
---------------------------
Here, we'll work with babel on a project to see how we can use it:
- Make an empty folder and open cmd in it.
- npm init
- npm install --save-dev @babel/core @babel/cli @babel/preset-env
- npm install @babel/polyfill
- Create a config file called .babelrc and add a preset @babel/preset-env:
``` json
{
	"presets": [ 
		"@babel/preset-env"
	]
}
```
- Create this dir structure:
	- root folder
		- src
			- index.js
		- dist
- Put any es6 code (for example, and arrow function) inside index.js
- Add a script to run babel to package.json, this script will:
	- run babel.
	- specify where the source code is (As argument of the cmd), which will be src.
	- specify where the destination should be (as argument of the cmd), which will be dest.
``` json
{
	... the rest of package.json
	
	"scripts": {
		"babel": "./node_modules/.bin/babel src -d dist"
	}
	
	... the rest of package.json
}
```
- npm run babel
- Check the dest folder, you'll see a code file named index.js where the same code you wrote in index.js is translated (or, transpiled) into es5 code.

Babel in webpack:
-----------------
Here we'll see how we can use babel with webpack:
- Make another new folder.
- npm install -D @babel/core @babel/preset-env babel-loader 
- npm install -D webpack webpack-cli path (you can combine this with the previous command)
- npm install @babel/polyfill
- Create the same dir structure with all the things we made last time (including the es6 code).
- .babelrc:
``` json
{
	"presets": [
		[
			"@babel/preset-env", {
				"modules": false,
				"targets": {
					"browsers": [
						"last 2 Chrome versions",
						"last 2 Firefox versions",
						"last 2 Safari versions",
						"last 2 iOS versions",
						"last 1 Android version",
						"last 2 ChromeAndroid version",
						"ie 11"
					]
				}
			}
		]
	]
}
```
	- "modules": false, this means that we're telling babel to NOT handle JS modules, because webpack will handle that.
	- "targets", this is how we specify which browsers we'll support.
- Create the webpack configuration file "webpack.config.js":
``` js
const path = require("path");

module.exports = {
	context: __dirname,
	entry: "./src/index.js",
	output: {
		path: path.resolve(__dirname, "dist"),
		filename: "main.js"
	},
	module: {
		rules: [
			{
				test: /\.js$/,
				exclude: /node_modules/,
				user: "babel-loader"
			}
		]
	}
}
``` 
- Add this script in package.json:
``` json
{
	...
	
	"scripts": {
		"dev": "webpack --watch --mode=development",
		"prod": "webpack --mode=production"
	}

	...
}
```
- npm run dev
- Check the dist folder.
