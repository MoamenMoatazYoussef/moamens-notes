# Webpack
Webpack is a static module bundler for JS apps.
It builds a dependency graph that maps each module your project needs, then generates *bundles*.

Dependency graph: https://webpack.js.org/concepts/dependency-graph/

Webpack normally doesn't need configuration, but it can be configured through the file ***webpack.config.js***.

Main pillars of webpack:
- Entry
- Output
- Loaders
- Plugins
- Mode
- Browser compatability
## Basic Concepts
### Entry
We specify a certain module in our JS project as Entry, typically a file.js

This file will be the starting point where webpack will start building its dependency graph. Think of it as the root of the graph.

We can define entry points in the config file (webpack.config.js) in many ways:
1. Single Entry, like this:
``` js
module.exports = {
  entry: {
    main: './path/to/my/entry/file.js'
  }
};
```

Or, shorthand version:
``` js
module.exports = {
  entry: './path/to/my/entry/file.js'
};
```

***Note:*** you can pass an array to entry, this is useful if you want to inject many dependent files together and graph their dependencies in one chunk.

2. Object Entry:
``` js
module.exports = {
  entry: {
    app: './src/app.js',
    adminApp: './src/adminApp.js'
  }
};
```
This is a more scalable way i.e. you can reuse these configurations and combine them with other configurations.

Then, different configurations are merged using webpack-merge: https://github.com/survivejs/webpack-merge

This may be needed for something like this:
``` js
module.exports = {
  entry: {
    pageOne: './src/pageOne/index.js',
    pageTwo: './src/pageTwo/index.js',
    pageThree: './src/pageThree/index.js'
  }
};
```

In a multi-page app, the server will fetch a new HTML every time. We can create bundles of code that is shared across pages. Use exactly ONE entry point per HTML document.

### Output
This is how webpack emits the bundles it creates and how to name them.
The default is ./dist/main.js for the main output file and ./dist directory for any other files.

Unlike entry points, only one output configuration is allowed.
``` js
module.exports = {
  output: {
    filename: 'bundle.js',
  }
};
```

If we have different entry points, and would like to save each "chunk" in a separate file, we should use substitutions:
``` js
module.exports = {
  entry: {
    app: './src/app.js',
    search: './src/search.js'
  },
  output: {
    filename: '[name].js',
    path: __dirname + '/dist'
  }
};
```

Check this to know more about substitutions: https://webpack.js.org/configuration/output/#outputfilename

### Loaders
Loaders are transformations made on the source code of a module. We can use these to preprocess the files as we import them in our modules.

We can use them for things like:
- converting TypeScript to JS.
- inline images as data URLs.
- import CSS files directly from JS modules.

Look in that code, notice the rules section, we make a test with a regex to find css and ts files, then use css-loader and ts-loader on each group of files respectively.
``` js
module.exports = {
  module: {
    rules: [
      { test: /\.css$/, use: 'css-loader' },
      { test: /\.ts$/, use: 'ts-loader' }
    ]
  }
};
```

There are three ways to use Loaders:
1. Config file: specify them in the webpack config file.
The above example is like that.
We use the object module.rules.
They execute from top to bottom if we define an array of loaders to add one one form of file, like this:
``` js
module.exports = {
  module: {
    rules: [
      {
        test: /\.css$/,
        use: [
          // style-loader
          { loader: 'style-loader' },
          // css-loader
          {
            loader: 'css-loader',
            options: {
              modules: true
            }
          },
          // sass-loader
          { loader: 'sass-loader' }
        ]
      }
    ]
  }
};
```
The style-loader executes first, then the css-loader, then the sass-loader.


2. Inline: specify them in import statements.
In your code, like this:
``` js
import Styles from 'style-loader!css-loader?modules!./styles.css';
```

Notice that we separated the loader with "!".

3. CLI: pass them as arguments within a command.
``` sh
webpack --module-bind jade-loader --module-bind 'css=style-loader!css-loader'
```

**Features of loaders** <br/>
- Loaders can be chained. Webpack expects the output of the last loader to be pure JS.
- They can be sync or async.
- They run in node.js and can use all its features.
- Can be configures with the *options* object.

### Plugins
A plugin is a JS object that has a method called *apply*, this method is called by the webpack compiler, so these JS objects can access the compilation lifecycle.
``` js
// It's recommended that you use a constant to store the plugin's name, the plugin's name should be camelCase or PascalCase
const pluginName = 'ConsoleLogOnBuildWebpackPlugin';

class ConsoleLogOnBuildWebpackPlugin {
  apply(compiler) {
    compiler.hooks.run.tap(pluginName, compilation => {
      console.log('The webpack build process is starting!!!');
    });
  }
}

module.exports = ConsoleLogOnBuildWebpackPlugin;
```

How to use plugins: in the config file:
``` js
module.exports = {
  entry: './path/to/my/entry/file.js',
  output: {
    filename: 'my-first-webpack.bundle.js',
    path: path.resolve(__dirname, 'dist')
  },
  module: {
    rules: [
      {
        test: /\.(js|jsx)$/,
        use: 'babel-loader'
      }
    ]
  },
  plugins: [
    new webpack.ProgressPlugin(), // this is used to access built-in webpack plugins
    new HtmlWebpackPlugin({template: './src/index.html'}) // installed by npm, the HtmlWebpackPlugin plugin takes an Object argument
  ]
};
```

### Mode
mode is an option in the config that specifies the current working mode of the project: dev, testing, production.


For more stuff about all the concepts of webpack e.g. more Loaders, plugins, etc. Check out the github page:
https://github.com/webpack/webpack

## Tutorial
### Creating a basic project using webpack
We'll create a JS project using webpack.
- Make sure Node.js is installed.
- Make a new directory.
- cd in the new directory.
- initialize an npm project:
``` sh
npm init
```
- open a cmd:
``` sh
npm install webpack
npm install webpack-cli
```

- Inside the directory, create these files and folders:
	- ./index.html
	- ./src/index.js

index.html:
``` html
<!doctype html>
<html>
  <head>
    <title>Getting Started</title>
    <script src="https://unpkg.com/lodash@4.16.6"></script>
  </head>
  <body>
    <script src="./src/index.js"></script>
  </body>
</html>
```

src/index.js:
``` js
function component() {
  const element = document.createElement('div');

  // Lodash, currently included via a script, is required for this line to work
  element.innerHTML = _.join(['Hello', 'webpack'], ' ');

  return element;
}

document.body.appendChild(component());
```

- Then, open the package.json file and make sure the package is private, and remove the "main" line, this is to prevent accidental publishing of our code:
``` json
{
    "name": "webpack-demo-1",
    "version": "1.0.0",
    "description": "",
    "private": true,
    "scripts": {
      "test": "echo \"Error: no test specified\" && exit 1"
    },
    "keywords": [],
    "author": "",
    "license": "ISC",
    "dependencies": {
      "webpack": "^4.41.2",
      "webpack-cli": "^3.3.10"
    }
  }
  
```

Notice that there are implicit dependencies between the ```<script>``` tags: index.js depends on lodash.
So, we need to load lodash, because index.js doesn't really declare a need to load lodash, it just assumes that the variable ```_``` exists.

- Modify the directory structure, move index.html into: ./dist/index.html
- install lodash using npm or yarn.
- Remove the line ```<script src="https://unpkg.com/lodash@4.16.6"></script>``` from ./dist/index.html. And change the other script tag to ```<script src="main.js"></script>```.
- add ```import _ from 'loadsh';``` to ./src/index.js at the start of the file.
- Now, run ```npx webpack```, this will take the script ./src/index.js as an entry point, run webpack, and generate ./dist/main.js.
- Open ./dist/index.html normally in a browser, you should see 'Hello webpack'.


Behind the scenes, webpack transpiles the code to ES5, so now older browsers can run it. But notice that webpack only transpiles import and export statements, along with something called Module API (https://webpack.js.org/api/module-methods/). Other ES5+ features should be transpiled by another transpiler e.g. Babel or Buble, both can be loaded in webpack as Loaders.

Now let's create a config file:
- Create this file in the project's root: webpack.config.js.
- Write this in it:
``` js
const path = require('path');

module.exports = {
	entry: './src/index.js',
	output: {
		filename: 'main.js',
		path: path.resolve(__dirname, 'dist'),
	},
};
```

- Run the command ```npx webpack``` but add to it the flag ```--config webpack.config.js```.


For ease of use, let's add an npm script to run webpack:
- Modify package.json in the *scripts* section:
``` json
  {
    "name": "webpack-demo",
    "version": "1.0.0",
    "description": "",
    "scripts": {
      "test": "echo \"Error: no test specified\" && exit 1",
      "build": "webpack --config webpack.config.js"
    },
    "keywords": [],
    "author": "",
    "license": "ISC",
    "devDependencies": {
      "webpack": "^4.20.2",
      "webpack-cli": "^3.1.2"
    },
    "dependencies": {
      "lodash": "^4.17.5"
    }
  }
```
Now, if you run ```npm run build```, it will run ```npx webpack``` behind the scenes, easily.

Nice :)

### Asset Management
We'll incorporate some assets e.g. images, etc. to see how we can handle them in webpack.
You can use gulp or grunt to process any assets and move them from /src to /dist and /build directories.
Webpack can do this too, and it can dynamically bundle them, so we can avoid bundling modules not in use.

Besides js files, webpack can also include any other type of file, and it has a loader to handle that type of file.

Let's try it, let's load some css:
- Change the /dist/index.html file:
``` html
  <!doctype html>
  <html>
    <head>
		<title>Asset Management</title>
    </head>
    <body>
		<script src="bundle.js"></script>
    </body>
  </html>
```

- Change the config file, change the output filename to *bundle.js*.
- To load css, we need two loaders: style-loader, and css-loader, so:
	- install both using npm or yarn.
	- add them to the webpack.config.file:
``` js
const path = require('path');

module.exports = {
	entry: './src/index.js',
	output: {
		filename: 'main.js',
		path: path.resolve(__dirname, 'dist'),
	},
	module: {
		rules: [
			{
				test: /\.css$/,
				use: ['style-loader', 'css-loader']
			}
		]
	}
};
```

Now, we can import our css files directly in js, like ```import "./this/file.css";```.
- Create this file /src/style.css
``` css
.hello {
	color: red;
}
```
- Then change the /src/index.js file like this:
``` js
import _ from 'lodash';
import "./styles.css";

function component() {
    const element = document.createElement('div');
  
    // Lodash, currently included via a script, is required for this line to work
    element.innerHTML = _.join(['Hello', 'webpack'], ' ');
	element.classList.add("hello");
  
    return element;
  }
  
  document.body.appendChild(component());
```
- Build the project using webpack, or the script ```npm run build```.
- Open the html file and see the output, the font should be red.

Nice :)

Loading images:
- install file-loader.
- add the loader and rules in the config file:
``` js
const path = require('path');

module.exports = {
	entry: './src/index.js',
	output: {
		filename: 'main.js',
		path: path.resolve(__dirname, 'dist'),
	},
	module: {
		rules: [
			{
				test: /\.css$/,
				use: ['style-loader', 'css-loader']
			},
			{
				test: /\.(png|svg|jpg|gif)$/,
				use: ['file-loader']
			}
		]
	}
};
```
- add any image to /src folder, call it icon.png for example.
- in the /src/index.js, import it like this:
``` js
import Icon from './icon.png';
```
And add it inside the function after the last statement we added:
``` js
const myIcon = new Image();
myIcon.src = Icon;

element.appendChild(myIcon);
```
Webpack will load the image in the page.

We can do that in css too:
- Add this to the src/styles.css file:
``` css
.hello {
	color: red;
	background: url('./icon.png');
}
```

- Build the application.
- Open the html page, you should see your image both as background and as an element under the text.

Nice :)

Check out the image-webpack-loader and url-loader, they can help minify and optimize images for better performance.

We can load fonts using file-loader:
- Add rules in the config file, the test should include file extensions woff, woff2, eot, ttf, otf. The loader to use is file-loader.
- Google for times new roman.ttf, download any file.
- name the file tnr-italic.ttf
- put it in the /src directory.
- in the src/styles.css, add the fonts:
``` css
@font-face {
	font-family: 'MyFont';
	src: url('./tnr-italic.ttf'), format('ttf');
	font-weight: 600;
}

.hello {
	/* the rest of this class */
	font-family: 'MyFont';
}
```
- Build and open the page, see the font.

Nice :)

We can load JSON, CSV, TSV, XML, and other data serialization files:
- install csv-loader and xml-loader.
- add loader rules for extensions csv and tsv, use csv-loader.
- add loader rules for extensions xml, use xml-loader.
- add a file /src/data.xml
``` xml
<note>
  <to>Mary</to>
  <from>John</from>
  <heading>Reminder</heading>
  <body>Call Cindy on Tuesday</body>
</note>
```
- import it in the /src/index.js file ```import Data from './data.xml';``` and ```console.log``` it.
- Build and run, open the page, see the console output.

Nice :)

### Output Management
Our project will grow as we go along, and we'll need to bundle a lot of files and output a lot of bundle files. How do we manage the output? Let's try bundling TWO files and outputting TWO files, then using both files in our page.
- Add /src/print.js 
``` js
export default function printMe() {
  console.log('I get called from print.js!');
}
```
- Then import and use that function in /src/index.js
- Update dist/index.html so that webpack splits our entries:
``` html
<!doctype html>
<html>
  <head>
    <title>Webpack Demo 1</title>
	<script src="./print.bundle.js"></script>
  </head>
  <body>
    <script src="./app.bundle.js"></script>
  </body>
</html>
```
- Modify the config file to:
	- Add new entry points: app, print.
	- Make the filename become [name].bundle.js
``` js
const path = require('path');

module.exports = {
	entry: {
		app: "./src/index.js",
		print: "./src/print.js"
	},
	output: {
		filename: '[name].bundle.js',
		path: path.resolve(__dirname, 'dist'),
	},
	module: {
		rules: [
			{
				test: /\.css$/,
				use: ['style-loader', 'css-loader']
			},
			{
				test: /\.(png|svg|jpg|gif)$/,
				use: ['file-loader']
			},
			{
				test: /\.(woff|woff2|eot|tff|otf)$/,
				use: ['file-loader']
			}
		]
	}
};
```
- build and run.
- Open the page, check the console.

Nice :)

It would be hard to manage the index.html file manually as the project grows, so there are some plugins to help with this.
HtmlWebpackPlugin: https://github.com/jantimon/html-webpack-plugin
Template for the plugin: https://github.com/jaketrent/html-webpack-template
- First, install it.
- Then, add it to the config file:
``` js
const path = require('path');
const HtmlWebpackPlugin = require('html-webpack-plugin');

module.exports = {
	entry: {
		app: "./src/index.js",
		print: "./src/print.js"
	},
	plugins: [
		new HtmlWebpackPlugin({
			title: "My Webpack Demo",
		}),
	],
	output: {
		filename: '[name].bundle.js',
		path: path.resolve(__dirname, 'dist'),
	},
	module: {
		rules: [
			{
				test: /\.css$/,
				use: ['style-loader', 'css-loader']
			},
			{
				test: /\.(png|svg|jpg|gif)$/,
				use: ['file-loader']
			},
			{
				test: /\.(woff|woff2|eot|tff|otf)$/,
				use: ['file-loader']
			}
		]
	}
};
```
- This plugin will output its own index.html file, even if we created one, it will override it.
- So try to build and run now, then open the page and see the output.

Nice :) 

As you have probably noticed, the dist folder is becoming a mess, webpack puts output in it but doesn't track which files in it are in use by the project.
So, it's a good practice to clean the dist folder before each build.
We'll use a plugin clean-webpack-plugin: https://www.npmjs.com/package/clean-webpack-plugin
- install it
- add it to the config file, it's not the default export of clean-webpack-plugin files, and it takes no arguments.
- run and build.
- You'll see that the previous main.js file was removed, in fact ALL of the files in dist were removed and only the ones webpack outputs are created again, main.js isn't in use anymore.

Nice :)

#### Manifest
You can further manage the output by using the manifest that webpack keeps to track how all the modules map to the output bundles.
Check out this WebpackManifestPlugin: https://github.com/danethurber/webpack-manifest-plugin
Read more about the manifest here: https://webpack.js.org/concepts/manifest/

### Development
Now we'll learn about setting a dev environment using webpack.
Note: DO NOT USE WHAT IS EXPLAINED HERE IN PRODUCTION.

First, set the mode to development in the config file.
``` js
module.exports = {
	mode: 'development',
	...
}
```

Now we'll learn about the webpack features we can use in development:
#### Source maps:
When bundling code using webpack, tracking errors is difficult because any error will point to bundle files, not entry files.
To solve this, JS offers "source maps", which map the output code to the original source code.
More about source maps here: https://blog.teamtreehouse.com/introduction-source-maps
Let's use a source map:
- in the config file, add the new key "devtool" to module.exports, and give it the value 'inline-source-maps', a basic source map to use in basic projects.
- Now let's create a bug to debug, in the print.js change the console.log to "cosnole.log", swap the 's' with the 'n'.
- Build and run, then see the console output.
- Check the stack trace of the error, you'll find that it points to print.js, not the bundle files.

Nice :)

#### Choosing a dev tool
It may become a hassle to manually build and run the project every time we change it.
Webpack offers 3 options to solve this:
- Watch Mode
- webpack-dev-server
- webpack-dev-middleware

1. Watch Mode: this is a feature that makes webpack 'watch' the files in the dependency graph, if one of them changes, webpack automatically recompiles.
	- Add w new npm script to package.json, called "watch", and give it the value "webpack --watch".
	- npm run watch
	- Then, fix the bug we created earlier in print.js.
	- Refresh the page, don't build and run.
	- You'll see that the code is automatically recompiled.
	
	Nice :)
	
2. webpack-dev-server: this is a simple web server that auto-refreshes, basically it does exactly what Watch Mode does, but instead of you manually refreshing, it auto refreshes too, so you just change the code.
	- install webpack-dev-server
	- add it to the config file like this:
	``` js
	devServer: {
		contentBase: './dist'
	}
	```
	- Since webpack-dev-server is a web server, it doesn't write output files after compiling, it keeps them in memory and serves them as if they were real files on the server's root path. If you want webpack-dev-server to look for bundle files in another path, use the publicPath option in devServer, check it out here: https://webpack.js.org/configuration/dev-server/#devserverpublicpath-
	- add a script to package.json "start", the value is "webpack-dev-server --open"
	- npm start
	- Notice that now the dev server operates on localhost:8080.
	- Change the code, and just save it, don't even refresh the page.
	- Notice that it will refresh it automatically.
	
	Nice :)
	
	webpack-dev-server comes with a LOT of configurations, check the docs here: https://webpack.js.org/configuration/dev-server/
	
	Usually, webpack-dev-server is the most used devServer.
	
3. webpack-dev-middleware: this emits files processed by webpack to a server, so it's used in webpack-dev-server internally. But we can use it separately with another server. We'll try to use *express* server.
	- install webpack-dev-middleware and express.
	- in the config file, in the module.exports.output, add publicPath and give it "/" as value.
	- create a new file in the root, server.js
``` js
const express = require('express');
const webpack = require('webpack');
const webpackDevMiddleware = require('webpack-dev-middleware');

const app = express();
const config = require('./webpack.config.js');
const compiler = webpack(config);

// Tell express to use the webpack-dev-middleware and use the webpack.config.js
// configuration file as a base.
app.use(webpackDevMiddleware(compiler, {
  publicPath: config.output.publicPath,
}));

// Serve the files on port 3000.
app.listen(3000, function () {
  console.log('Example app listening on port 3000!\n');
});
```
	- Add an npm script to run the server, "node server.js".
	- npm run server.
	- open localhost:3000

Real nice :)

### Code Splitting
Webpack is awesome, one of the reasons it's awesome is because it allows you to split your code into different bundles, these bundles can be loaded on demand or in parallel.
This means we can prioritize our resource load, this can help us shorten load time a lot.

How do we do this?
- Entry points: manually split code using them.
- SplitChunksPlugin to split code into chunks. https://webpack.js.org/plugins/split-chunks-plugin/
- Dynamic imports: split code using inline function calls.

Let's go over those methods:
1. Entry points: easiest and most intuitive, but manual and has some problems.
	Let's try it anyway:
	- Create src/another-module.js.
	- Put this code inside it:
	``` js
	import _ from 'lodash';

	console.log(
	_.join(['Another', 'module', 'loaded!'], ' ')
	);
	```
	- Add a new entry point "another" in your config file.
	- Build and run.
	- Notice that lodash has been imported TWICE.
	That's nice and all, but well, if two files import one module, the module will be imported TWICE. Duplication.
	
2. SplitChunksPlugin:
	This plugin allows us to extract common dependencies into an entry chunk or a new chunk, to prevent duplication.
	- Add to the config file module.exports.optimization.splitChunks.chunks = 'all'.
	- This should separate the lodash into a new bundle and used that bundle ONCE.
	
	Other plugins and loaders for splitting code: bundle-loader, promise-loader, mini-css-extract-plugin.

3. Dynamic Imports:
	This is achieved by ```import()```, or ```require.ensure```. ```import()``` is recommended and conforms to ECMAScript, as ```require.ensure``` is legacy and webpack-specific.
	import() uses Promises, so if you'll use pre-es6, use polyfills: https://github.com/stefanpenner/es6-promise or https://github.com/taylorhakes/promise-polyfill 
	- Remove the "another" entry point from the config file.
	- Remove the optimization entry from the config file.
	- Add this to the config file: module.exports.output.chunkFilename = '[name].bundle.js'
	- Remove the src/another-module.js file.
	- In the src/index.js, we'll use dynamic imports:
``` js
function GetComponent() {
    return import(/* webpackChunkName: "lodash" */ 'lodash').then( ({ default: _ }) => {
		const element = document.createElement('div');
		element.innerHTML = _.join(['Hello', 'webpack'], ' ');
		return element
	}).catch(error => "An error occured:" + error);
 }

getComponent().then(component =>   
  document.body.appendChild(component())
 }
```

Or you can use async:
``` js
async function GetComponent() {
	const element = document.createElement('div');
	const default = await import(/* webpackChunkName: "lodash" */ 'lodash');
	element.innerHTML = _.join(['Hello', 'webpack'], ' ');
	return element
 }
```
- In the /* */ we put inline directives, this can help us.
- The *default* argument is because of this: https://medium.com/webpack/webpack-4-import-and-commonjs-d619d626b655
- Build and run.
- It will work, but now it loads lodash WHEN it's needed, not at compile time, so loading time is much less.

Nice :)

The power of import() also is in that we can use dynamic expressions to load things according to a computed variable, check this: https://webpack.js.org/api/module-methods/#dynamic-expressions-in-import

Prefetching and Preloading:
- We can use the inline directives webpackPrefetch: true, to load a module that we expect to use in the future.
- We can also use webpackPreload: true.
- What's the difference, you ask?
	- a Preloaded chunk loads parallel to the parent, the prefetched starts after the parent.
	- A preloaded chunk downloads instantly, a prefetched one is downloaded when the browser is idle.
	- A preloaded chunk should be requested instantly by the parent, a prefetched can be used later.

Be careful when using preload, it can hurt performance.

Bundle Analysis:
- You can analyze where each module have ended up in your split bundles, you can use this official tool: https://github.com/webpack/analyse
- Or a lot of other tools, you can find them here: https://webpack.js.org/guides/code-splitting/

### Caching
Browsers cache the data retrieved from the /dist folder to make performance better.
But how do we configure this in webpack?

1. Output filenames: we can use a certain 'substitution' to define the names of output files.
	Remember the '[name].bundle.js' we used? this [name] is a 'substitution', basically the original source filename replaces these brackets, that's how we produce different bundles.
	Some substitutions are special:
		[contenthash]: adds a unique hash based on the content of the file.
		[name]: gets replaced by the name of the file.
	Let's try it:
	- in the config file, add [contenthash] to the output.filename.
	- Build and run, then inspect the /dist filenames, you'll see an added hash code.
	NICEEEE :)
	
2. Extracting Boilerplace: if we build again, the filenames might change, that's because the hashcode is computed from the contents of the file AND a boilerplate that ensures that the hashcode constantly changes. 
	The boilerplate is specifically the Runtime and Manifest.
	Remember the *SplitChunksPlugin*? it splits modules into separate bundles. We can optimize this using an option *optimization.runtimeChunk* = ```single```, this creates a single runtime bundle for all chunks.
	- In the config file, set optimization.runtimeChunk = 'single'.
	- Build and run, see the output, see the ```runtime``` bundle.
	- It's also a best practice to separate vendor code e.g. lodash, react, etc. into a separate chunk because they are less likely to change than our local source code. So clients only request our source code without requesting vendor code.
	- This can be dune by setting optimization.splitChunks.cacheGroups.vendor:
		- test: regex pointing to node_modules
		- name: 'vendors'
		- chunks: 'all'
	- Add this in your config file.
	- build and run. See the vendors.[hashcode].js output.
	
	What if we added a new module?
		- The hashcode of files change based on ```module.id```, which is incremented based on resolving order.
		- This means that adding new modules will make the vendors hashcode change despite not changing anything in it, now the client has to fetch it.
		- So, to fix this, add optimization.moduleIds = 'hashed' to the config file.
		- Build and run.
		- Make any change in the code or add a new file and write a dummy function.
		- Build and run.
		- Compare the hashcode of the vendors bundle in both builds, they should be the same.
		
	Nice :)

### Bundling JS libraries
Webpack can be used to bundle JS libraries.
- Create a new project:
	- the config file.
	- package.json (npm init or yarn init)
	- /src/index.js
	- /src/ref.json
- Install webpack, webpack-cli, and lodash.
- Write this in the ref.json:
``` json
[
  {
    "num": 1,
    "word": "One"
  },
  {
    "num": 2,
    "word": "Two"
  },
  {
    "num": 3,
    "word": "Three"
  },
  {
    "num": 4,
    "word": "Four"
  },
  {
    "num": 5,
    "word": "Five"
  },
  {
    "num": 0,
    "word": "Zero"
  }
]
```