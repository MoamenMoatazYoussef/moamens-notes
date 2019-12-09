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