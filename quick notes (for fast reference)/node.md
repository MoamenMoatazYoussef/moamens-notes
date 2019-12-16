# Node quick notes
## architecture
V8 Js engine + some more modules to help with backend dev.

## How node works
A single thread can handle many requests in an async non-blocking manner.
With an event queue for messages that wait for the non-blocking thread.

Node is good for highly scalable, data intensive, realt-time apps, but is NOT good for cpu-intensive apps or apps that require a lot of cpu computations.

## Global stuff
There are some objects that are global in Node:
- console.log
- setTimeout, clearTimeout, setInterval, and clearInterval

In browsers, we have the *window* object that represents the global scope.
In Node, we don't have that *window* object.
Instead, we have the ```global``` object.

A small difference between both is that if we declare a variable using ```var```, in browsers the variable will be in the ```window``` object's scope. **But**, in node, the ```global``` object does NOT contain the variable in its scope.

## Node modules
The above difference between ```global``` and ```window``` is because node relies on an architecture of ***Modules***.
Every file in Node is a module, the variables and functions defined inside it are scoped inside it i.e. they are private to it.

If you want to use stuff inside the module from outside it, you need to explicitly export and use it.
We have an object in node called ```module```. it's **NOT** a global object. But it contains an object *exports*, anything we put in that will be public and exported outside the module.

### How to create a module and load it in another module
- Create a new file *logger.js* in your directory.
- inside it:
``` js
var url = 'http://mylogger.io/log';

function log(message) {
	console.log(message);
}

module.exports = {
	log,
	url
}
```
- then, create another file *app.js*:
``` js
const logger = require('./logger.js');
logger.log("Everyday I'm shufflin'");
```
- then open a terminal in the directory:
``` sh
node app.js
```
- Check the output :)

***Note:*** if you're exporting just a function, you can use ```module.exports = log```, then requiring the module will give yo ua function so you can just ```logger("Everyday I'm shufflin'")```.

## Node wrapper function
When you execute a node module, node wraps it in this IFFE:
``` js
(function (exports, require, module, __filename, __dirname) {
	// your app here
})();
```
It's kinda more complex than that, but it's the same idea.
Any module is wrapped in an IFFE like that.

This function is called the ***Module Wrapper Function***.
You can access these arguments btw. Try adding this to app.js:
``` js
console.log(__filename);
```
Then run again and see the output.

## Node Modules
Some of the most important Node modules are: File System, HTTP, OS, Path, Process, Query Strings, Steam.

Check out all the modules here:
https://nodejs.org/dist/latest-v12.x/docs/api/

### Path Module
In App.js:
``` js
const path = require('path');
const pathObject = path.parse(__filename);
console.log(pathObject);
```

### OS Module
In App.js:
``` js
const os = require('os');
console.log(os.totalmem());
console.log(os.freemem());
```

### FS Module
We'll use a method readdir, to read the filenames inside the current directory.
Notice that we have readdir and readdirSync, a synchronous blocking form of the readdir. 
In App.js:
``` js
const fs = require('fs');
const files = fs.readdir("./", (err, files) => {
	if(err) {
		throw err;
	} else {
		console.log(files);
	}
});
```

### Events Module
Events are a core concept of Node. Node is event-driven. 
We'll use Events module.
The Events module returns a *Class*, that's why:
- we usually name it in Pascal case.
- we need to instantiate it using the ```new``` keyword.

We can add arguments to events e.g ID, url, etc.
We can send them directly because they will be destructured, but it's better to encapsulate them in an object.
In app.js:
``` js
const EventEmitter = require('events');
const emitter = new EventEmitter();

// we'll listen to a specific event.
// on() and addListener() are the same
emitter.on('messageLogged', () => {
	console.log("I received an event.");
})

// the method 'emit' fires an event
// the 2nd param is optional, it's the arguments we want to send with the event
emitter.emit('messageLogged', { message: "Party rock is in the house tonight" });
```
### Events across modules
To call an event and listen to it from another module, make a class that extends EventEmitter, inside it define your logic and emit your events using this. keyword, then export it, then use it in the other module.
logger.js:
``` js
const EventEmitter = require('events');

class Logger extends EventEmitter {
	log(message) {
		console.log("Logger:", message);
		this.emit('messageLogged', { message: message });
	}	
}

module.exports = Logger;
```

app.sj
``` js
const Logger = require("./logger");
const logger = new Logger();

logger.on('messageLogged', args => {
	console.log("------");
	console.log("Party rock is in the house tonight");
	console.log(args.message);
});

logger.log("Everybody just have a good time");
```

### HTTP Module
``` js
const http = require('http');
const PORT = 3000;

// the http.Server object extends net.Server, which itself extends EventEmitter
const server = http.createServer();

server.on('connection', socket => {
	console.log("New connection...")
});

server.listen(PORT);

console.log("Listening on port ", PORT, "...");
```
- Run it, then open localhost:3000, then see the console log.

Also, we can pass a callback function to createServer():
``` js
function(req, res) {
	if(req.url === "/") {
		// do something here
		res.write("Let's get REDICULOUS");
		res.end();
	}
	
	if(req.url === "/api/courses") {
		res.write(JSON.stringify([1, 2, 3]));
		res.end();
	}
	
	//as you can see, we can do the routing here
}
```

So, let's add this to app.js:
``` js
const http = require('http');
const PORT = 3000;

// the http.Server object extends net.Server, which itself extends EventEmitter
const server = http.createServer((req, res) => {
	if(req.url === "/") {
		res.write("Let's get REDICULOUS");
		res.end();
	}
	
	if(req.url === "/api/courses") {
		res.write(JSON.stringify([1, 2, 3]));
		res.end();
	}
});

server.listen(PORT);

console.log("Listening on port ", PORT, "...");
```
- $ node app.js
- Open localhost:3000 and localhost:3000/api/courses

## RESTful services with Express.js
We know what these are.

### Express.js
As we've seen, building routes like that is not scalable.
We'll use a framework: ***Express.js***

- Make a new folder for a new project
- npm init
- npm i express
- add a file index.js
``` js
const express = require('express');
const app = express();

app.get("/", (req, res) => {
    res.send("Get crazy, loud, get wild in the crowd");
});

app.get("/api/courses", (req, res) => {
    res.send(JSON.stringify(["I'm laid back", "I'm feeling this", "Come on baby", "Let's get rediculous"]));
});

app.listen(3000, () => console.log("Yeah baby, listening on port 3000 \\o/"))
});
```
- node index.js
- open localhost:3000 and localhost:3000/api/courses

Nice :)

### Nodemon
Node Monitor, a devtool that watches files, if we change one, it restarts the app. It's like webpack-dev-server.
- npm i nodemon
- Add a script to package.json: "nodemon" "npx nodemon index.js"
- npm run nodemon

### Environment variables
Instead of hard-coding the port, we'll use the *process* object to get environment variables for the port. (Environment variable: variable provided by the OS and its value is assigned by OS)
``` js
const port = process.env.PORT || 3000; // 3000 is default, but not mandatory
```
Then, if we want to set that variable for our OS, we do this:
``` sh
$ export PORT=5000
```
In windows (Not tested):
``` sh
setx PORT 3000
```
index.js:
``` js
const express = require('express');
const app = express();

app.get("/", (req, res) => {
    res.send("Get crazy, loud, get wild in the crowd");
});

app.get("/api/courses", (req, res) => {
    res.send(JSON.stringify(["I'm laid back", "I'm feeling this", "Come on baby", "Let's get rediculous"]));
});

const PORT = process.env.PORT || 3000; // 3000 is default, but not mandatory

app.listen(PORT, () => console.log("Yeah baby, listening on port " + PORT + "\\o/"))
```

Now run the app (or don't, it will restart automatically thanks to nodemon), see the port you're listening to.

### Parameters in URL of routing
Add ```:``` then the name of the parameter.
In index.js, add this route:
``` js
app.get("/api/courses/:id", (req, res) => {
	res.send(req.params.id);
});
```
Now go to localhost:[whatever the port is]/api/courses/1 and see the output.

Nice :)

### Query String parameters
***Note:*** I'll write localhost:3000 from now on, if you have a different port, use it.

```localhost:[whatever the port is]/api/courses/1?sortBy=name``` <br/>
We want to get that sortBy query value.
In index.js in the last route we added, change the response:
``` js
app.get("/api/courses/:id", (req, res) => {
	res.send(req.query);
});
```

### Handling HTTP GET requests
What if we want to add a course?
First, let's define a dummy database, add this on top of the index.js
``` js
const courses = [
	{
		name: "c1",
		id: 1
	},
	{
		name: "c2",
		id: 2
	}, 
	{
		name: "c3",
		id: 3
	}
];
```

Then, in the new route we added, we'll get a course from the list and return it:
``` js
app.get("/api/courses/:id", (req, res) => {
	const course = courses.find(c => c.id === Number(req.params.id));
	if(!course) {
		res.status(404).send("Course not found.");
	}
	res.send(course);
});
```
Now go to localhost:3000/api/courses/1, then localhost:3000/api/courses/77.
Check the output.

Nice :)

### Handling POST requests
We need to enable JSON parsing, it's not enabled in express by default, so we'll add this on top:
``` js
app.use(express.json());
```
Essentially, express.json() returns a piece of middleware, then app uses it.

Then, add this route:
``` js
app.post("/api/courses", (req, res) => {
	const course = {
		id: courses.length + 1, // should be assigned by DB auto incremenet or something
		name: req.body.name
	};

	courses.push(course);
	res.status(201).send(course);
});
```
Use postman to try posting a course.

### Input validation
As a security best practice, NEVER trust what the client sends you. ALWAYS validate the input.
We will validate the name in the post mapping we added:
``` js
app.post("/api/courses", (req, res) => {

	if(!req.body.name){
		res.status(400).send("Name is required");
		return;
	}
	else if(req.body.name.length < 3) {
		res.status(400).send("Name should be minimum 3 characters");
		return;
	}

	const course = {
		id: courses.length + 1, // should be assigned by DB auto incremenet or something
		name: req.body.name
	};

	courses.push(course);
	res.status(200).send(course);
});
```

This would work, but we can make it easier by using a package:
``` sh
npm i joi
```
Then, require it. It returns a class, so make the variable Pascal Case.
``` js
const Joi = require('joi');
```

Then, we use something called *schema*, sort of a template that Joi will use to validate the input.
``` js
const schema = {
	name: Joi.string().min(3).required()
}
```

Then, we'll use that schema to validate the input like this:
``` js
const result = Joi.validate(req.body, schema);
```

This result object containes the information you need to know if there were errors or not.
We'll use result.error.
``` js
if(!result.error) {
	res.status(400).send(result.error);
	return;
}
```

To simplify it, use result.error.details[0].message or 

The index.js now looks like that:
``` js
const Joi = require('joi');
const express = require('express');
const app = express();

app.use(express.json());

const courses = [
	{
		name: "c1",
		id: 1
	},
	{
		name: "c2",
		id: 2
	}, 
	{
		name: "c3",
		id: 3
	}
];

app.get("/", (req, res) => {
    res.send("Get crazy, loud, get wild in the crowd");
});

app.get("/api/courses", (req, res) => {
    res.send(JSON.stringify(["I'm laid back", "I'm feeling this", "Come on baby", "Let's get rediculous"]));
});

app.get("/api/courses/:id", (req, res) => {
	const course = courses.find(c => c.id === Number(req.params.id));
	if(!course) {
		res.status(404).send("Course not found.");
	}
	res.send(course);
});

app.post("/api/courses", (req, res) => {
    const schema = {
        name: Joi.string().min(3).required()
    }

    const result = Joi.validate(req.body, schema);

    if(!result.error) {
        res.status(400).send(result.error);
        return;
    }
    
    const course = {
		id: courses.length + 1,
		name: req.body.name
	};

	courses.push(course);
	res.status(201).send(course);
});

const PORT = process.env.PORT || 3000;

app.listen(PORT, () => console.log("Yeah baby, listening on port " + PORT + "\\o/"))
```
Now cause an error and see the output.

Nice :)

### Handling PUT Requests
``` js
app.put('/api/courses/:id', (req, res) => {
	const course = courses.find(c => c.id === Number(req.params.id));
	
	if(!course) {
		res.status(404).send("Course not found.");
	}

    if(!result.error) {
        res.status(400).send(result.error);
        return;
	}

	course.name = req.body.name;

	res.send(course);
});
```

For clean code's sake, we'll define a function for validation i.e. the reused code:
``` js
function validateCourse(course) {
	const schema = {
        name: Joi.string().min(3).required()
    }

    return Joi.validate(course, schema);
}
```

Then use it in both the post 1 course and put 1 course routes:
``` js
app.post("/api/courses", (req, res) => {
    const { error } = validateCourse(req.body);

    if(error) {
        res.status(400).send(error.details[0].message);
        return;
	}
    
    const course = {
		id: courses.length + 1,
		name: req.body.name
	};

	courses.push(course);
	res.status(201).send(course);
});

app.put('/api/courses/:id', (req, res) => {
    const course = courses.find(c => c.id === Number(req.params.id));
    
    console.log(course);
	
	if(!course) {
		res.status(404).send("Course not found.");
	}

	const { error } = validateCourse(req.body);

    if(error) {
        res.status(400).send(error.details[0].message);
        return;
	}

	course.name = req.body.name;

	res.send(course);
});
```
Try updating a course from Postman.

Nice :)

### DELETE
``` js
app.delete('/api/courses/:id', (req, res) => {
    const course = courses.find(c => c.id === Number(req.params.id));
    
    console.log(course);
	
	if(!course) {
		res.status(404).send("Course not found.");
	}

	const index = courses.indexOf(course);
	courses.splice(index, 1);

	res.send(course);
});
```
Try a DELETE request from postman.

Real nice :)

### Advanced Topics in Express
#### Middleware
A Middleware function is a a function that takes a request, and either returns a response, or passes control to another function.

It's a function that's put in the MIDDLE of requesting and handling the request, hence, *middleware function*.

An example is what we've used already:
``` js
app.use(express.json());
```
i.e. Use the function express.json() as a middleware function, so the request goes first through that function, and the output of that function goes to the backend.

We can define our own *middleware functions* too, which are useful for cross-cutting concerns: logging, security, etc.

An express application is essentially a bunch of middleware functions. The routing is also middleware.

#### Creating a custom middleware function
First, we use ```app.use()```, and we pass it a function, that function will be a middleware function.

Example, add this to index.js:
``` js
app.use((req, res, next) => {
	console.log("I'm in the first middleware");
	next(); 
	// this is essential because we need to either respond to the client
	// or pass control to another middleware function, that's why we need
	// to call that next() function
	// otherwise, the request will hang there and no movement will happen...
});

// so, we'll add another middleware request

app.use((req, res, next) => {
	console.log("I'm in the second middleware");
	next(); 
});
```

Add these two expressions to index.js, then try anything from postman.
You'll see both console.logs in the console.
They are called in sequence of their adding.

Cleaning the code:
- Put each middleware function in a separate file/module.
ping.js
``` js
function Ping(req, res, next) {
    console.log("Ping");
    next();
}

module.exports = Ping;
```

pong.js
``` js
function Pong(req, res, next) {
    console.log("Pong");
    next();
}

module.exports = Pong;
```

index.js
``` js
const Joi = require('joi');
const express = require('express');
const app = express();

const ping = require('./middleware/ping');
const pong = require('./middleware/pong'); 

app.use(express.json());
app.use(ping);
app.use(pong);

// rest of the code
```

Now try it again.

Nice :)

#### Express Built-in Middleware
Express provides some built-in middleware functions for us.
json() is one of them.

Another one:
- urlencoded({ extended: true }): It parses requests with URL-encoded payload i.e. key=value pairs in the URL, and populates req.body with the output.
- static('dirname'): This is used to serve static files, the argument dirname should be a directory of the static files.

#### Express Third-party Middleware
There are a lot of them, like:
- helmet(): provides security by adding some HTTP headers to the requests.
- morgan(): logs HTTP requests.

Check more of them and their docs here:
https://expressjs.com/en/resources/middleware.html

### Environments
Dev, Test, or Prod?
We use an environment variable NODE_ENV
``` js
process.env.NODE_ENV
```
We can set it to dev, testing ,staging, production.
Try logging the variable in your index.js.

Another way:
``` js
app.get('env');
```
This has a default value 'development', but we can set it too.
This can be used to apply middleware only in some environments.
``` js
if(app.get('env') === 'development') {
	app.use(morgan('tiny'));
	console.log('Morgan enabled...');
}
```
Try to set NODE_ENV to 'production', and run the application again.

### Configuration
Storing config settings for the app and overriding them in each environment.
A popular package for this is ```rc```, not npmrc, just ***rc***.
Another one is ***config***.
- Install *config*.
``` sh
$ npm i config
```
- Create a new folder.
- in it, create default.json
``` json
{
	"name": "default config"
}
```
- Another file, development.json
``` json
{
	"name": "development config"
}
```
- if you want, you can do the same for testing, staging, production.

<h2> WARNING: DO NOT store sensitive info in the config file. </h2>
Instead, store them in *environment variables*, then map config variables to these environment variables.

### Debugging
The holy ```console.log``` is our hero in debugging.
But, sometimes you need a justice league, not a single hero.

So, we'll call for the justice league of debugging in Express: the Debug package.
- install debug
```
npm i debug
```
- require 'debug', this returns a function, call it immediately and pass to it a 'namespace', something like this: 'app:startup'.
``` js
const startupDebugger = require('debug')('app:startup');
```
- do the same, give it another namespace e.g. app:db
``` js
const dbDebugger = require('debug')('app:db');
```

-Now, whenever you need to write console.log, use the debugger functions we made instead, like this:
``` js
startupDebugger("I'm here");
startupDebugger("hobba el lala");
```
- Now, set the DEBUG environment variable to a namespace, this will make ONLY the debugging function defined in that namespace to work.
	- setting it to many namespaces (or using a wildcard) will enable different debuggers, and they'll be colored differently in the console. 
	- setting it to nothing turns of debugging. 
Windows:
``` sh
export DEBUG='app:startup'
```

### Templating engines for Express's JSON
We know templating engines, like ***Mustache***.
But we'll use another one: ***pug***.
- npm i pug
- index.js
``` js
app.set('view engine', 'pug');
```
- if you want to override the path to the templates (default value = "./views")
``` js
app.set('views', 'the path to the templates');
```
- create a file in the path to the templates, called *index.pug*
``` pug
html
	head
		title=title
	body
		h1=message
```
- back in index.js, in any of the route functions, instead of res.send, use res.render. The first argument is the name of the .pug file without the extension, the second argument is an object containing the properties we used in the pug file i.e. ```title```, ```message```.
``` js
res.render("index", { title="My awesome app", message="Hello again })
```

### Database & Authentication
Check out here:
https://expressjs.com/en/guide/database-integration.html
for info about database integration with Express and Node.

Express doesn't go into authentication, we'll explore that later outside of Express.

### How to structure Express apps
For every api, we want to have a separate module.
- Create a new folder called *routes*, inside it a file *courses.js*
	- require express in it.
	- cut all the route functions that have /api/courses, and paste them here.
	- but, instead of using ```app = express()```, we'll use ```express.router()``` object, also call it *router*. Replace all *app* instances with *router*.
	- export the router at the end.
``` js
const express = require('express');
const router = express.Router();

router.get("/api/courses", (req, res) => {
    res.send(courses);
});

router.get("/api/courses/:id", (req, res) => {
    const course = courses.find(c => c.id === Number(req.params.id));
    if (!course) {
        return res.status(404).send("Course not found.");
    }
    res.send(course);
});

router.post("/api/courses", (req, res) => {
    const {
        error
    } = validateCourse(req.body);

    if (error) {
        return res.status(400).send(error.details[0].message);
    }

    const course = {
        id: courses.length + 1,
        name: req.body.name
    };

    courses.push(course);
    res.status(201).send(course);
});

router.put('/api/courses/:id', (req, res) => {
    const course = courses.find(c => c.id === Number(req.params.id));

    console.log(course);

    if (!course) {
        return res.status(404).send("Course not found.");
    }

    const {
        error
    } = validateCourse(req.body);

    if (error) {
        return res.status(400).send(error.details[0].message);
    }

    course.name = req.body.name;

    res.send(course);
});

router.delete('/api/courses/:id', (req, res) => {
    const course = courses.find(c => c.id === Number(req.params.id));

    console.log(course);

    if (!course) {
        return res.status(404).send("Course not found.");
    }

    const index = courses.indexOf(course);
    courses.splice(index, 1);

    res.status(204).send(course);
});

module.exports = router;
```

- in index.js:
	- require the courses in a constant *coursesRouter*.
	- add this ```app.use('/api/courses', coursesRouter);```, this basically means: if the url has /api/courses in it, use the router you got from *courses*.
- Back in courses.js, remove /api/courses from all urls, replace them with /, express uses the router when /api/courses is involved anyway, so we don't need to write it.
- Also, move the *courses* constant variable, any needed requires. and the function validateCourse to the courses.js file:
``` js
const express = require('express');
const config = require('config');
const Joi = require('joi');
const router = express.Router();

const courses = [{
    name: "c1",
    id: 1
},
{
    name: "c2",
    id: 2
},
{
    name: "c3",
    id: 3
}
];

function validateCourse(course) {
    const schema = {
        name: Joi.string().min(3).required()
    }

    return Joi.validate(course, schema);
}

router.get("/", (req, res) => {
    res.send(courses);
});

router.get("/:id", (req, res) => {
    const course = courses.find(c => c.id === Number(req.params.id));
    if (!course) {
        return res.status(404).send("Course not found.");
    }
    res.send(course);
});

router.post("/", (req, res) => {
    const {
        error
    } = validateCourse(req.body);

    if (error) {
        return res.status(400).send(error.details[0].message);
    }

    const course = {
        id: courses.length + 1,
        name: req.body.name
    };

    courses.push(course);
    res.status(201).send(course);
});

router.put('/:id', (req, res) => {
    const course = courses.find(c => c.id === Number(req.params.id));

    console.log(course);

    if (!course) {
        return res.status(404).send("Course not found.");
    }

    const {
        error
    } = validateCourse(req.body);

    if (error) {
        return res.status(400).send(error.details[0].message);
    }

    course.name = req.body.name;

    res.send(course);
});

router.delete('/:id', (req, res) => {
    const course = courses.find(c => c.id === Number(req.params.id));

    console.log(course);

    if (!course) {
        return res.status(404).send("Course not found.");
    }

    const index = courses.indexOf(course);
    courses.splice(index, 1);

    res.status(204).send(course);
});

module.exports = router;
```

index.js
``` js
const express = require('express');
const config = require('config');
const Joi = require('joi');
const router = express.Router();

const courses = [{
    name: "c1",
    id: 1
},
{
    name: "c2",
    id: 2
},
{
    name: "c3",
    id: 3
}
];

function validateCourse(course) {
    const schema = {
        name: Joi.string().min(3).required()
    }

    return Joi.validate(course, schema);
}

router.get("/", (req, res) => {
    res.send(courses);
});

router.get("/:id", (req, res) => {
    const course = courses.find(c => c.id === Number(req.params.id));
    if (!course) {
        return res.status(404).send("Course not found.");
    }
    res.send(course);
});

router.post("/", (req, res) => {
    const {
        error
    } = validateCourse(req.body);

    if (error) {
        return res.status(400).send(error.details[0].message);
    }

    const course = {
        id: courses.length + 1,
        name: req.body.name
    };

    courses.push(course);
    res.status(201).send(course);
});

router.put('/:id', (req, res) => {
    const course = courses.find(c => c.id === Number(req.params.id));

    console.log(course);

    if (!course) {
        return res.status(404).send("Course not found.");
    }

    const {
        error
    } = validateCourse(req.body);

    if (error) {
        return res.status(400).send(error.details[0].message);
    }

    course.name = req.body.name;

    res.send(course);
});

router.delete('/:id', (req, res) => {
    const course = courses.find(c => c.id === Number(req.params.id));

    console.log(course);

    if (!course) {
        return res.status(404).send("Course not found.");
    }

    const index = courses.indexOf(course);
    courses.splice(index, 1);

    res.status(204).send(course);
});

module.exports = router;
```

Test it with postman, see the results :)

Do the whole process again, for "/", create a router  in a module *home.js*.

## Asynchronous JavaScript Programming
setTimeout, setInterval, both are async functions.
That is, they schedule a task to be performed some time in the future.
It doesn't wait for that point of time, it just schedules it, then control is transferred to the next statement.
``` js
console.log("before");
setTimeout(() => {
	console.log("Inside");
}, 2000);
console.log("after");
```
The output will be:
before
after
Inside

What's going to happen, is that *before* and *after* will immediately print, then after two seconds, the *Inside* will output.

***Note:*** This does **NOT** mean we have different threads, it's a single thread that doesn't wait for every single line of code to fully execute.

Node uses asynchronous programming a lot.

There are three (two really) ways to deal with async programming:
- Callbacks
- Promises
- Async/await (which is syntactic sugar, internally they use Promises)

### Callbacks
Let's start with this code:
``` js
console.log("Before");
const user = getUser(1);
console.log(user);
console.log("After")

function getUser(id) {
	setTimeout(() => {
		console.log("Retrieving user...");
		return {
			id: id,
			name: "User"
		};
	}, 2000);
}
```

To use callbacks to handle async programming, we do this:
``` js
console.log("Before");
getUser(1, user => console.log('User:', user));
console.log("After")

function getUser(id, callback) {
    setTimeout(() => {
        console.log("Retrieving user...");
        callback({
            id: id,
            name: "User"
        });
    }, 2000);
}
```
Execute this code, now you can access the user ONCE it's retrieved.

Let's add another function to execute once we get the user.
``` js
console.log("Before");
getUser(1, user => console.log('User:', user));
console.log("After")

function getUser(id, callback) {
    setTimeout(() => {
        console.log("Retrieving user...");
        const user = {
            id: id,
            name: "User"
        };
        callback(user);
        getUserRepos(user, repos => {
            repos.forEach(repo => console.log(repo));
         });
    }, 2000);
}

function getUserRepos(username, callback) {
    setTimeout((username) =>{
        console.log("Getting repos...");
        callback(['repo1', 'repo2', 'repo3']);
    }  , 2000);
}
```
That will execute fine and asynchronously, but there's a problem: in real-life, we would like to do a lot of things to data retrieved from the database, a lot of callbacks will be needed.

This means that we need to *nest* the calls to each function to ensure that they execute after each other, not asynchronously.

This creates some spaghetti code, known as *Callback Hell*, or more colorfully, *Christmas Tree Problem*.

A solution for this is *Named functions*, instead of anonymous functions or arrow functions (that are anonymous by default), we use named functions, functions with names.

How will that make a difference?
Look here:
``` js
console.log("Before");
getUser(1, displayUser);
console.log("After")

function getUser(id, callback) {
    setTimeout(() => {
        console.log("Retrieving user...");
        const user = {
            id: id,
            name: "User"
        };
        callback(user);
        getUserRepos(user, getRepos);
    }, 2000);
}

function displayUser(user) {
    console.log('User:', user);
}

function getRepos(repos) {
    repos.forEach(repo => console.log(repo));
}

function getUserRepos(username, callback) {
    setTimeout((username) => {
        console.log("Getting repos...");
        callback(['repo1', 'repo2', 'repo3']);
    }, 2000);
}
```
We separated the functions into named functions to be able to pass references to them.
This kinda solved the issue...

But, a hero comes to the rescue...*Promises*

### Promises
A Promise is an object that holds an eventual result of an async operation, either value or error.
It can be in one of these states:
- Pending: it's waiting for the async operation.
- Fulfilled: async operation finished successfully.
- Rejected: async operation threw an error.

Add a new file promise.js:
``` js
// the constructor takes two params:
// 1: a function that inside it we do async stuff
//      this function takes two args:
//      a. resolve: a function that executes if the async was successful
//      b. reject: the same but for failure
// 2: 
const p = new Promise(function(resolve, reject) {
    // here we write the async code
    setTimeout(() => {
        resolve(1);
    }, 2000);
});

// promises have a function .then(), which accepts a functio as argument, 
// and get passed the result of the promise as argument.
p.then(result => console.log(result))
```
Run it, and see how it runs.

Nice :)

We can also return an error if a problem happens, run this code:
``` js
const p = new Promise(function(resolve, reject) {
    setTimeout(() => {
        // resolve(1);
        reject(new Error("my error"));
    }, 2000);
});

p.then(result => console.log(result)).catch(err => console.log(err));
```
The catch block will run because we rejected the promise i.e. we raised an error.

***Pro tip:*** Modify function that takes callbacks as arguments so that they return Promises, then pass the callbacks in the .then() function.

Let's modify our old code in index.js
``` js
console.log("Before");
getUser(1)
	.then(user => displayUser(user))
    .then(user => getUserRepos(user))
    .then(repos => getRepos(repos));

console.log("After")

function getUser(id) {
    return new Promise((resolve, reject) => {
        setTimeout(() => {
            console.log("Retrieving user...");
            const user = {
                id: id,
                name: "User"
            };
            resolve(user);
        }, 2000);
    });
}

function displayUser(user) {
    console.log('User:', user);
}

function getRepos(repos) {
    repos.forEach(repo => console.log(repo));
}

function getUserRepos(username) {
    return new Promise((resolve, reject) => {
        setTimeout((username) => {
            console.log("Getting repos...");
            resolve(['repo1', 'repo2', 'repo3']);
        }, 2000);
    })
}
```
Notice that we could chain .then() calls, because each function returns a Promise.
And it's a LOT cleaner than callbacks, this is how we do it:
``` js
console.log("Before");
getUser(1)
	.then(user => displayUser(user))
    .then(user => getUserRepos(user))
	.then(repos => getRepos(repos))
	.catch(error => console.log(`Error: ${error}`));

console.log("After")

// rest of the code
```
As a best practice, always include the .catch() function, and always use new Error on rejection, not just a message, so that it displays the call stack.

### Creating Settled Promises
Sometimes we need that, like in unit tests.
``` js
const p = Promise.resolve(...);
p.then(() => { // do what you want here });
```

replace with .reject() & .catch() if you want an already rejected Promise.

### Parallel Promises
``` js
const p1 = new Promise(resolve => {
	setTimeout(() => {
		console.log("Async 1");
		resolve(1);
	}, 2000);
});

const p2 = new Promise(resolve => {
	setTimeout(() => {
		console.log("Async 2");
		resolve(1);
	}, 2000);
});

// kick off both promises
// returns a new Promise after all the passed promises are resolved 
// the result of that returned Promise will be an array of results of the passed promises
Promise.all([p1, p2])
	.then(result => console.log(result))
	.catch(err => console.log("error", err.message));
	// this fires off if any of the passed Promises are rejected
	// i.e. the returned promise isn't resolved except if ALL promises passed to it are resolved

// Some more convenience methods:
Promise.race([p1, p2])
	.then(//)
	.catch(//);

// This promise is resolved and returned as soon as ONE of the promises resolves
// it's like a race between these promises
```
They run in parallel (actually, they *seem* to run in parallel).
This is NOT real multuithreading, this is a single thread that handles both async operations.

### Async and Await
This is syntactic suger that uses Promises in the background.
It's good because it makes us write async code that is kinda similar to sync/normal code.

``` js
// this is a normal call to a function
normalFunction();

// but we can't do this for an async function because it will execute in sync which is wrong
asyncFunction();

// so, we can do this
await asyncFunction();

// it actually executes in an async way, while looking like a normal sync call
// so, in the index.js code, the first proposal that didn't work was like this:
const user = getUser(1);

// now, it can work, but like this:
const user = await getUser(1);
```

That is what ```await``` is for.
But what about ```async``` ? <br/>

This keyword is used to indicate that this function contains an async operation i.e. an operation that uses ```await```.
This is a requirement by JS engines, a compiler's way of knowing that this uses ```await```.

``` js
// here's a normal function that has sync code inside it
function normalWrapper() {
	const a = normalFunction();
}

// here's a function that uses await inside it
async function asyncWrapper() {
	const a = await asyncFunction();
}
```
async functions return a ```Promise<void>``` (Because async & await use Promises in the background).
So, ```await``` doesn't actually *wait*, it is converted to a set Promise that execute in parallel (kinda), like what we've seen before.

Also, we don't use the .catch() method, we use a normal try/catch block
``` js
async function asyncWrapper() {
	try {
		const a = await asyncFunction();
	} catch(error) {
		console.log(`Error: ${error}`);
	}
}
```