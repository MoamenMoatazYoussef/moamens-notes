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

In browsers, we have the _window_ object that represents the global scope.
In Node, we don't have that _window_ object.
Instead, we have the `global` object.

A small difference between both is that if we declare a variable using `var`, in browsers the variable will be in the `window` object's scope. **But**, in node, the `global` object does NOT contain the variable in its scope.

## Node modules

The above difference between `global` and `window` is because node relies on an architecture of **_Modules_**.
Every file in Node is a module, the variables and functions defined inside it are scoped inside it i.e. they are private to it.

If you want to use stuff inside the module from outside it, you need to explicitly export and use it.
We have an object in node called `module`. it's **NOT** a global object. But it contains an object _exports_, anything we put in that will be public and exported outside the module.

### How to create a module and load it in another module

- Create a new file _logger.js_ in your directory.
- inside it:

```js
var url = "http://mylogger.io/log";

function log(message) {
  console.log(message);
}

module.exports = {
  log,
  url
};
```

- then, create another file _app.js_:

```js
const logger = require("./logger.js");
logger.log("Everyday I'm shufflin'");
```

- then open a terminal in the directory:

```sh
node app.js
```

- Check the output :)

**_Note:_** if you're exporting just a function, you can use `module.exports = log`, then requiring the module will give yo ua function so you can just `logger("Everyday I'm shufflin'")`.

## Node wrapper function

When you execute a node module, node wraps it in this IFFE:

```js
(function(exports, require, module, __filename, __dirname) {
  // your app here
})();
```

It's kinda more complex than that, but it's the same idea.
Any module is wrapped in an IFFE like that.

This function is called the **_Module Wrapper Function_**.
You can access these arguments btw. Try adding this to app.js:

```js
console.log(__filename);
```

Then run again and see the output.

## Node Modules

Some of the most important Node modules are: File System, HTTP, OS, Path, Process, Query Strings, Steam.

Check out all the modules here:
https://nodejs.org/dist/latest-v12.x/docs/api/

### Path Module

In App.js:

```js
const path = require("path");
const pathObject = path.parse(__filename);
console.log(pathObject);
```

### OS Module

In App.js:

```js
const os = require("os");
console.log(os.totalmem());
console.log(os.freemem());
```

### FS Module

We'll use a method readdir, to read the filenames inside the current directory.
Notice that we have readdir and readdirSync, a synchronous blocking form of the readdir.
In App.js:

```js
const fs = require("fs");
const files = fs.readdir("./", (err, files) => {
  if (err) {
    throw err;
  } else {
    console.log(files);
  }
});
```

### Events Module

Events are a core concept of Node. Node is event-driven.
We'll use Events module.
The Events module returns a _Class_, that's why:

- we usually name it in Pascal case.
- we need to instantiate it using the `new` keyword.

We can add arguments to events e.g ID, url, etc.
We can send them directly because they will be destructured, but it's better to encapsulate them in an object.
In app.js:

```js
const EventEmitter = require("events");
const emitter = new EventEmitter();

// we'll listen to a specific event.
// on() and addListener() are the same
emitter.on("messageLogged", () => {
  console.log("I received an event.");
});

// the method 'emit' fires an event
// the 2nd param is optional, it's the arguments we want to send with the event
emitter.emit("messageLogged", {
  message: "Party rock is in the house tonight"
});
```

### Events across modules

To call an event and listen to it from another module, make a class that extends EventEmitter, inside it define your logic and emit your events using this. keyword, then export it, then use it in the other module.
logger.js:

```js
const EventEmitter = require("events");

class Logger extends EventEmitter {
  log(message) {
    console.log("Logger:", message);
    this.emit("messageLogged", { message: message });
  }
}

module.exports = Logger;
```

app.sj

```js
const Logger = require("./logger");
const logger = new Logger();

logger.on("messageLogged", args => {
  console.log("------");
  console.log("Party rock is in the house tonight");
  console.log(args.message);
});

logger.log("Everybody just have a good time");
```

### HTTP Module

```js
const http = require("http");
const PORT = 3000;

// the http.Server object extends net.Server, which itself extends EventEmitter
const server = http.createServer();

server.on("connection", socket => {
  console.log("New connection...");
});

server.listen(PORT);

console.log("Listening on port ", PORT, "...");
```

- Run it, then open localhost:3000, then see the console log.

Also, we can pass a callback function to createServer():

```js
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

```js
const http = require("http");
const PORT = 3000;

// the http.Server object extends net.Server, which itself extends EventEmitter
const server = http.createServer((req, res) => {
  if (req.url === "/") {
    res.write("Let's get REDICULOUS");
    res.end();
  }

  if (req.url === "/api/courses") {
    res.write(JSON.stringify([1, 2, 3]));
    res.end();
  }
});

server.listen(PORT);

console.log("Listening on port ", PORT, "...");
```

- \$ node app.js
- Open localhost:3000 and localhost:3000/api/courses

## RESTful services with Express.js

We know what these are.

### Express.js

As we've seen, building routes like that is not scalable.
We'll use a framework: **_Express.js_**

- Make a new folder for a new project
- npm init
- npm i express
- add a file index.js

```js
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

Instead of hard-coding the port, we'll use the _process_ object to get environment variables for the port. (Environment variable: variable provided by the OS and its value is assigned by OS)

```js
const port = process.env.PORT || 3000; // 3000 is default, but not mandatory
```

Then, if we want to set that variable for our OS, we do this:

```sh
$ export PORT=5000
```

In windows (Not tested):

```sh
setx PORT 3000
```

index.js:

```js
const express = require("express");
const app = express();

app.get("/", (req, res) => {
  res.send("Get crazy, loud, get wild in the crowd");
});

app.get("/api/courses", (req, res) => {
  res.send(
    JSON.stringify([
      "I'm laid back",
      "I'm feeling this",
      "Come on baby",
      "Let's get rediculous"
    ])
  );
});

const PORT = process.env.PORT || 3000; // 3000 is default, but not mandatory

app.listen(PORT, () =>
  console.log("Yeah baby, listening on port " + PORT + "\\o/")
);
```

Now run the app (or don't, it will restart automatically thanks to nodemon), see the port you're listening to.

### Parameters in URL of routing

Add `:` then the name of the parameter.
In index.js, add this route:

```js
app.get("/api/courses/:id", (req, res) => {
  res.send(req.params.id);
});
```

Now go to localhost:[whatever the port is]/api/courses/1 and see the output.

Nice :)

### Query String parameters

**_Note:_** I'll write localhost:3000 from now on, if you have a different port, use it.

`localhost:[whatever the port is]/api/courses/1?sortBy=name` <br/>
We want to get that sortBy query value.
In index.js in the last route we added, change the response:

```js
app.get("/api/courses/:id", (req, res) => {
  res.send(req.query);
});
```

### Handling HTTP GET requests

What if we want to add a course?
First, let's define a dummy database, add this on top of the index.js

```js
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

```js
app.get("/api/courses/:id", (req, res) => {
  const course = courses.find(c => c.id === Number(req.params.id));
  if (!course) {
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

```js
app.use(express.json());
```

Essentially, express.json() returns a piece of middleware, then app uses it.

Then, add this route:

```js
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

```js
app.post("/api/courses", (req, res) => {
  if (!req.body.name) {
    res.status(400).send("Name is required");
    return;
  } else if (req.body.name.length < 3) {
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

```sh
npm i joi
```

Then, require it. It returns a class, so make the variable Pascal Case.

```js
const Joi = require("joi");
```

Then, we use something called _schema_, sort of a template that Joi will use to validate the input.

```js
const schema = {
  name: Joi.string()
    .min(3)
    .required()
};
```

Then, we'll use that schema to validate the input like this:

```js
const result = Joi.validate(req.body, schema);
```

This result object containes the information you need to know if there were errors or not.
We'll use result.error.

```js
if (!result.error) {
  res.status(400).send(result.error);
  return;
}
```

To simplify it, use result.error.details[0].message or

The index.js now looks like that:

```js
const Joi = require("joi");
const express = require("express");
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
  res.send(
    JSON.stringify([
      "I'm laid back",
      "I'm feeling this",
      "Come on baby",
      "Let's get rediculous"
    ])
  );
});

app.get("/api/courses/:id", (req, res) => {
  const course = courses.find(c => c.id === Number(req.params.id));
  if (!course) {
    res.status(404).send("Course not found.");
  }
  res.send(course);
});

app.post("/api/courses", (req, res) => {
  const schema = {
    name: Joi.string()
      .min(3)
      .required()
  };

  const result = Joi.validate(req.body, schema);

  if (!result.error) {
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

app.listen(PORT, () =>
  console.log("Yeah baby, listening on port " + PORT + "\\o/")
);
```

Now cause an error and see the output.

Nice :)

### Handling PUT Requests

```js
app.put("/api/courses/:id", (req, res) => {
  const course = courses.find(c => c.id === Number(req.params.id));

  if (!course) {
    res.status(404).send("Course not found.");
  }

  if (!result.error) {
    res.status(400).send(result.error);
    return;
  }

  course.name = req.body.name;

  res.send(course);
});
```

For clean code's sake, we'll define a function for validation i.e. the reused code:

```js
function validateCourse(course) {
  const schema = {
    name: Joi.string()
      .min(3)
      .required()
  };

  return Joi.validate(course, schema);
}
```

Then use it in both the post 1 course and put 1 course routes:

```js
app.post("/api/courses", (req, res) => {
  const { error } = validateCourse(req.body);

  if (error) {
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

app.put("/api/courses/:id", (req, res) => {
  const course = courses.find(c => c.id === Number(req.params.id));

  console.log(course);

  if (!course) {
    res.status(404).send("Course not found.");
  }

  const { error } = validateCourse(req.body);

  if (error) {
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

```js
app.delete("/api/courses/:id", (req, res) => {
  const course = courses.find(c => c.id === Number(req.params.id));

  console.log(course);

  if (!course) {
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

It's a function that's put in the MIDDLE of requesting and handling the request, hence, _middleware function_.

An example is what we've used already:

```js
app.use(express.json());
```

i.e. Use the function express.json() as a middleware function, so the request goes first through that function, and the output of that function goes to the backend.

We can define our own _middleware functions_ too, which are useful for cross-cutting concerns: logging, security, etc.

An express application is essentially a bunch of middleware functions. The routing is also middleware.

#### Creating a custom middleware function

First, we use `app.use()`, and we pass it a function, that function will be a middleware function.

Example, add this to index.js:

```js
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

```js
function Ping(req, res, next) {
  console.log("Ping");
  next();
}

module.exports = Ping;
```

pong.js

```js
function Pong(req, res, next) {
  console.log("Pong");
  next();
}

module.exports = Pong;
```

index.js

```js
const Joi = require("joi");
const express = require("express");
const app = express();

const ping = require("./middleware/ping");
const pong = require("./middleware/pong");

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

```js
process.env.NODE_ENV;
```

We can set it to dev, testing ,staging, production.
Try logging the variable in your index.js.

Another way:

```js
app.get("env");
```

This has a default value 'development', but we can set it too.
This can be used to apply middleware only in some environments.

```js
if (app.get("env") === "development") {
  app.use(morgan("tiny"));
  console.log("Morgan enabled...");
}
```

Try to set NODE_ENV to 'production', and run the application again.

### Configuration

Storing config settings for the app and overriding them in each environment.
A popular package for this is `rc`, not npmrc, just **_rc_**.
Another one is **_config_**.

- Install _config_.

```sh
$ npm i config
```

- Create a new folder.
- in it, create default.json

```json
{
  "name": "default config"
}
```

- Another file, development.json

```json
{
  "name": "development config"
}
```

- if you want, you can do the same for testing, staging, production.

<h2> WARNING: DO NOT store sensitive info in the config file. </h2>
Instead, store them in *environment variables*, then map config variables to these environment variables.

### Debugging

The holy `console.log` is our hero in debugging.
But, sometimes you need a justice league, not a single hero.

So, we'll call for the justice league of debugging in Express: the Debug package.

- install debug

```
npm i debug
```

- require 'debug', this returns a function, call it immediately and pass to it a 'namespace', something like this: 'app:startup'.

```js
const startupDebugger = require("debug")("app:startup");
```

- do the same, give it another namespace e.g. app:db

```js
const dbDebugger = require("debug")("app:db");
```

-Now, whenever you need to write console.log, use the debugger functions we made instead, like this:

```js
startupDebugger("I'm here");
startupDebugger("hobba el lala");
```

- Now, set the DEBUG environment variable to a namespace, this will make ONLY the debugging function defined in that namespace to work. - setting it to many namespaces (or using a wildcard) will enable different debuggers, and they'll be colored differently in the console. - setting it to nothing turns of debugging.
  Windows:

```sh
export DEBUG='app:startup'
```

### Templating engines for Express's JSON

We know templating engines, like **_Mustache_**.
But we'll use another one: **_pug_**.

- npm i pug
- index.js

```js
app.set("view engine", "pug");
```

- if you want to override the path to the templates (default value = "./views")

```js
app.set("views", "the path to the templates");
```

- create a file in the path to the templates, called _index.pug_

```pug
html
	head
		title=title
	body
		h1=message
```

- back in index.js, in any of the route functions, instead of res.send, use res.render. The first argument is the name of the .pug file without the extension, the second argument is an object containing the properties we used in the pug file i.e. `title`, `message`.

```js
res.render("index", { title="My awesome app", message="Hello again })
```

### Database & Authentication

Check out here:
https://expressjs.com/en/guide/database-integration.html
for info about database integration with Express and Node.

Express doesn't go into authentication, we'll explore that later outside of Express.

### How to structure Express apps

For every api, we want to have a separate module.

- Create a new folder called _routes_, inside it a file _courses.js_ - require express in it. - cut all the route functions that have /api/courses, and paste them here. - but, instead of using `app = express()`, we'll use `express.router()` object, also call it _router_. Replace all _app_ instances with _router_. - export the router at the end.

```js
const express = require("express");
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
  const { error } = validateCourse(req.body);

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

router.put("/api/courses/:id", (req, res) => {
  const course = courses.find(c => c.id === Number(req.params.id));

  console.log(course);

  if (!course) {
    return res.status(404).send("Course not found.");
  }

  const { error } = validateCourse(req.body);

  if (error) {
    return res.status(400).send(error.details[0].message);
  }

  course.name = req.body.name;

  res.send(course);
});

router.delete("/api/courses/:id", (req, res) => {
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

- in index.js: - require the courses in a constant _coursesRouter_. - add this `app.use('/api/courses', coursesRouter);`, this basically means: if the url has /api/courses in it, use the router you got from _courses_.
- Back in courses.js, remove /api/courses from all urls, replace them with /, express uses the router when /api/courses is involved anyway, so we don't need to write it.
- Also, move the _courses_ constant variable, any needed requires. and the function validateCourse to the courses.js file:

```js
const express = require("express");
const config = require("config");
const Joi = require("joi");
const router = express.Router();

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

function validateCourse(course) {
  const schema = {
    name: Joi.string()
      .min(3)
      .required()
  };

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
  const { error } = validateCourse(req.body);

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

router.put("/:id", (req, res) => {
  const course = courses.find(c => c.id === Number(req.params.id));

  console.log(course);

  if (!course) {
    return res.status(404).send("Course not found.");
  }

  const { error } = validateCourse(req.body);

  if (error) {
    return res.status(400).send(error.details[0].message);
  }

  course.name = req.body.name;

  res.send(course);
});

router.delete("/:id", (req, res) => {
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

```js
const express = require("express");
const config = require("config");
const Joi = require("joi");
const router = express.Router();

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

function validateCourse(course) {
  const schema = {
    name: Joi.string()
      .min(3)
      .required()
  };

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
  const { error } = validateCourse(req.body);

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

router.put("/:id", (req, res) => {
  const course = courses.find(c => c.id === Number(req.params.id));

  console.log(course);

  if (!course) {
    return res.status(404).send("Course not found.");
  }

  const { error } = validateCourse(req.body);

  if (error) {
    return res.status(400).send(error.details[0].message);
  }

  course.name = req.body.name;

  res.send(course);
});

router.delete("/:id", (req, res) => {
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

Do the whole process again, for "/", create a router in a module _home.js_.

## Asynchronous JavaScript Programming

setTimeout, setInterval, both are async functions.
That is, they schedule a task to be performed some time in the future.
It doesn't wait for that point of time, it just schedules it, then control is transferred to the next statement.

```js
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

What's going to happen, is that _before_ and _after_ will immediately print, then after two seconds, the _Inside_ will output.

**_Note:_** This does **NOT** mean we have different threads, it's a single thread that doesn't wait for every single line of code to fully execute.

Node uses asynchronous programming a lot.

There are three (two really) ways to deal with async programming:

- Callbacks
- Promises
- Async/await (which is syntactic sugar, internally they use Promises)

### Callbacks

Let's start with this code:

```js
console.log("Before");
const user = getUser(1);
console.log(user);
console.log("After");

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

```js
console.log("Before");
getUser(1, user => console.log("User:", user));
console.log("After");

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

```js
console.log("Before");
getUser(1, user => console.log("User:", user));
console.log("After");

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
  setTimeout(username => {
    console.log("Getting repos...");
    callback(["repo1", "repo2", "repo3"]);
  }, 2000);
}
```

That will execute fine and asynchronously, but there's a problem: in real-life, we would like to do a lot of things to data retrieved from the database, a lot of callbacks will be needed.

This means that we need to _nest_ the calls to each function to ensure that they execute after each other, not asynchronously.

This creates some spaghetti code, known as _Callback Hell_, or more colorfully, _Christmas Tree Problem_.

A solution for this is _Named functions_, instead of anonymous functions or arrow functions (that are anonymous by default), we use named functions, functions with names.

How will that make a difference?
Look here:

```js
console.log("Before");
getUser(1, displayUser);
console.log("After");

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
  console.log("User:", user);
}

function getRepos(repos) {
  repos.forEach(repo => console.log(repo));
}

function getUserRepos(username, callback) {
  setTimeout(username => {
    console.log("Getting repos...");
    callback(["repo1", "repo2", "repo3"]);
  }, 2000);
}
```

We separated the functions into named functions to be able to pass references to them.
This kinda solved the issue...

But, a hero comes to the rescue..._Promises_

### Promises

A Promise is an object that holds an eventual result of an async operation, either value or error.
It can be in one of these states:

- Pending: it's waiting for the async operation.
- Fulfilled: async operation finished successfully.
- Rejected: async operation threw an error.

Add a new file promise.js:

```js
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
p.then(result => console.log(result));
```

Run it, and see how it runs.

Nice :)

We can also return an error if a problem happens, run this code:

```js
const p = new Promise(function(resolve, reject) {
  setTimeout(() => {
    // resolve(1);
    reject(new Error("my error"));
  }, 2000);
});

p.then(result => console.log(result)).catch(err => console.log(err));
```

The catch block will run because we rejected the promise i.e. we raised an error.

**_Pro tip:_** Modify function that takes callbacks as arguments so that they return Promises, then pass the callbacks in the .then() function.

Let's modify our old code in index.js

```js
console.log("Before");
getUser(1)
  .then(user => displayUser(user))
  .then(user => getUserRepos(user))
  .then(repos => getRepos(repos));

console.log("After");

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
  console.log("User:", user);
}

function getRepos(repos) {
  repos.forEach(repo => console.log(repo));
}

function getUserRepos(username) {
  return new Promise((resolve, reject) => {
    setTimeout(username => {
      console.log("Getting repos...");
      resolve(["repo1", "repo2", "repo3"]);
    }, 2000);
  });
}
```

Notice that we could chain .then() calls, because each function returns a Promise.
And it's a LOT cleaner than callbacks, this is how we do it:

```js
console.log("Before");
getUser(1)
  .then(user => displayUser(user))
  .then(user => getUserRepos(user))
  .then(repos => getRepos(repos))
  .catch(error => console.log(`Error: ${error}`));

console.log("After");

// rest of the code
```

As a best practice, always include the .catch() function, and always use new Error on rejection, not just a message, so that it displays the call stack.

### Creating Settled Promises

Sometimes we need that, like in unit tests.

```js
const p = Promise.resolve(...);
p.then(() => { // do what you want here });
```

replace with .reject() & .catch() if you want an already rejected Promise.

### Parallel Promises

```js
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

They run in parallel (actually, they _seem_ to run in parallel).
This is NOT real multuithreading, this is a single thread that handles both async operations.

### Async and Await

This is syntactic suger that uses Promises in the background.
It's good because it makes us write async code that is kinda similar to sync/normal code.

```js
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

That is what `await` is for.
But what about `async` ? <br/>

This keyword is used to indicate that this function contains an async operation i.e. an operation that uses `await`.
This is a requirement by JS engines, a compiler's way of knowing that this uses `await`.

```js
// here's a normal function that has sync code inside it
function normalWrapper() {
  const a = normalFunction();
}

// here's a function that uses await inside it
async function asyncWrapper() {
  const a = await asyncFunction();
}
```

async functions return a `Promise<void>` (Because async & await use Promises in the background).
So, `await` doesn't actually _wait_, it is converted to a set Promise that execute in parallel (kinda), like what we've seen before.

Also, we don't use the .catch() method, we use a normal try/catch block

```js
async function asyncWrapper() {
  try {
    const a = await asyncFunction();
  } catch (error) {
    console.log(`Error: ${error}`);
  }
}
```

## MongoDB

It's a Document (NoSQL) DB, so we don't have tables, schemas, views, records, columns, etc.
We don't design a schema or design, we simply store JSON objects in MongoDB.

### Mongoose

An npm package, a simple api to work with MongoDB.

- New project, install mongoose

```sh
npm init -y
npm i mongoose
```

- create index.js

```js
const mongoose = require("mongoose");

// Here, we connect and use the connection string (ideally, it comes from a config file)
// mongodb://<hostname>/<database_name>
// note that we haven't created this *playground* database
// but when we connect and write something to it, mongoDB auto-creates it

// this method returns a promise, so we can use .then()
mongoose
  .connect("mongodb://localhost/playground")
  .then(() => console.log("Connected to MongoDb..."))
  .catch(error => console.log("Error:", error));
```

Run it, and check the output.

Nice :)

### Schema

_Schema_ is the shape of data within a database.
In SQL, _Schema_ is the Tables, how they're defined, their columns, the relations, etc.
But, in MongoDB we don't have Tables and stuff, we have:

- Collection: like a table.
- Documents: like a record/entry/row in a table.

Unlike tables and entries, each document is an array of key/value pairs.

In MongoDB, there is no _Schema_ per se. But in _mongoose_, there is a _schema_ object, that we use to define the shape of the mongoDB collections and documents.

To create a schema, we use mongoose.Schema constructor, and we pass to it an object that contains properties and their values are **DATA TYPES** in JS, we specify the fields and the data they should accept.

The types that are available are: String, Number, Date, Buffer (for binary data), Boolean, ObjectID (to assign unique IDs), Array.

For example, we want to define a schema for any Course we store in the database:

```js
const mongoose = require("mongoose");

mongoose
  .connect("mongodb://localhost/playground")
  .then(() => console.log("Connected to MongoDb..."))
  .catch(error => console.log("Error:", error));

const courseSchema = new mongoose.Schema({
  name: String, // we have a property "name", whose value should be "string"
  author: String,
  tags: [String], // the value should be an array of strings
  date: {
    // the value should be an object that has *type* as a property
    type: Date,
    default: Date.now // this is a default value for the type if no value was specified
  },
  isPublished: Boolean
});
// now, courseSchema defines how courses are structured in the database
```

Now, we need to complie this schema into a Model, so that we create a Class from its shape, and any new Course entry should be an instance of that Class.

Add this after the previous code:

```js
// this function creates a model and returns a class based on it
const Course = mongoose.model("Course", courseSchema);

// let's try to make an instance of that class
const defenseAgainstDarkArts = new Course({
  name: "Defense Against Dark Arts",
  author: "Severus Snape",
  tags: ["dark arts", "hogwarts"]
});
```

Check out the tags property, see? The document here can be a complex object.
In SQL databases we don't have that, if we want to model that in a SQL database we'd need a table for courses, a table for tags, and a join table for them since they are in a many-to-many relationship.

That's where MongoDB is powerful, we just create an encapsulated object and store it
in the database.
That's why MongoDB (and similar ones like CouchDB and DynamoDB) is called _Schemaless_ they have no Schema

### How to save that course

We'll use course.save() function.
It's an async function, we're dealing with an async operation since we're accessing the file system and saving in it.
And it returns the unique ID of the newly saved course.

```js
const defenseAgainstDarkArtsID = await defenseAgainstDarkArts.save();
```

We can refactor the code like this:

```js
async function createCourse() {
  const defenseAgainstDarkArts = new Course({
    name: "Defense Against Dark Arts",
    author: "Severus Snape",
    tags: ["dark arts", "hogwarts"]
  });
  const defenseAgainstDarkArtsID = await defenseAgainstDarkArts.save();
  console.log("Newly saved ID: ", defenseAgainstDarkArtsID);
}

createCourse();
```

Now, run the code, see the ID printed on the screen.

Nice :)

### How to query a document in MongoDB

Querying is also an async operation, let's create a function to query:

```js
async function getCourses() {
  // we'll use the Course class to query, it has a set of functions that start with "find"
  // find: returns the whole list of documents
  // findById: gets one by id
  // and a lot more...
  const coursesDocumentQuery = await Course.find();
  console.log(coursesDocumentQuery);
}

getCourses();
```

We can add filters to the find() method like this:

```js
async function getCourses() {
  const coursesDocumentQuery = await Course.find({
    author: "Severus Snape",
    isPublished: true
  });
  console.log(coursesDocumentQuery);
}

getCourses();
```

Try both, see the result.

We can also get specific properties by building specific queries.
The find() method returns a DocumentQuery object, we can use some convenience methods of that object like:

- limit(10): limits the amount of returned results to 10 for example.
- sort({ name: 1 }): we pass an object here, where we specify properties to use in sorting, and give them 1 for ascending, or -1 for descending.
- select({ name: 1, tags: 1 }): we pass an object here, where we specify which specific properties we want.

#### Complex Queries - Comparison Operators

We can use some operators for these queries too, like:

- eq: ===
- ne: !==
- gt: >
- gte: >=
- lt & lte
- in: is one of specific values we provide, like an array.
- nin: not in

Assume we have a _price_ property in our Course object, what if we want courses whose price is greater than 10?

```js
// check here, see how we used an object in the price: value to indicate that
// we want the price to be greater than 10
const courses = await Course.find({ price: { $gt: 10 } });

// we can combine different conditions, like AND
// let's get the courses whose price is greater than 10 but less than or equal to 20
const courses = await Course.find({ price: { $gt: 10, $lte: 20 } });

// what if we want specific values?
// like courses whose price is either 10, 15, or 20
// here's the use of the operator *in*
const courses = await Course.find({ price: { $in: [10, 15, 20] } });
```

#### Complex Queries - Logical Operators

We can actually use the standard logical operators, you know OR, AND, etc.:

- or
- and

```js
// instead of passing something to find(), we'll use another method or()
// and in it, we'll pass an array of filters, if the result passes through
// at least one of these filters, it will be returned
// i.e. we are ORing the filters
const courses = await Course.find().or([
  { author: "Severus Snape" },
  { isPublished: true }
]);

// the same concept using and() method, but ANDing instead of ORing
```

#### Complex Queries - Regexes

We can us Regexes in queries to make things easier when using string filters.

```js
// get courses whose:
// - name ends with "Arts",
// - author starts with "Severus",
const courses = await Course.find({
  name: /Arts$/,
  author: /^Severus/
});

// get courses whose name contains "Dark"
// also, this query is case-insensitive i.e. it can contain dark or Dark or dArK, etc.
const courses = await Course.find({
  name: /.*Drts,*/i
});
```

The idea here is that we can pass regexes to the filters.
Check the JS Regex documentation for a LOT more possibilities for regexes.

### Counting

Very easy, use .count();

```js
const courses = await Course.find().count();

console.log(courses);
```

This will print the number of results that match your query.

### Pagination

Using .skip() method to split the results into _pages_.

We want to divide our results into pages of 10 results each.
To go to a certain page, page number n, we need to skip all the documents in the previous page, page n-1, so we pass a formula to the skip() function to do that calculation.

```js
const pageNumber = 2;
const pageSize = 10;

const courses = await Course.find()
  .skip((pageNumber - 1) * pageSize)
  .limit(pageSize);
```

#### Updating Documents

Basically, there are two ways to update:

1. Query first:
   - Find the document to update.
   - Update it.
   - Save it back.

```js
async function updateCourse(id, data) {
  const course = await Course.findById(id);
  if (!course) return;

  course = { ...course, name: data.name, author: data.author };
  // Another way of updating is course.set() method, check its docs out

  const updatedCourse = await course.save();
  console.log(updatedCourse);
}

const theId = 1;
const updateData = {
  name: "How to become a software wizard",
  author: "Harry Potter"
};
updateCourse(theId, updateData);
```

2. Update first:
   - Update directly in database.
   - [optional] get the updated document.

```js
async function updateCourse(id, data) {
  // the first argument is a filter
  // the second is the update data using the mongoDB Update operators
  // mongoDB provides operators to modify directly in the database
  // check the documentation, search for "Update operators"
  const result = await Course.update(
    { _id: id },
    {
      $set: data
    }
  );
}

const theId = 1;
const updateData = {
  name: "How to become a software wizard",
  author: "Harry Potter"
};
updateCourse(theId, updateData);
```

You can also use findByIdAndUpdate(id, { ... }) to get the updated document, instead of update(). The first argument is just going to be the ID, not a filter.

#### Removing Documents

```js
async function removeCourse(id) {
  // deletes the first occurance of whatever matches the filter

  Course.deleteOne({ _id: id });
}

const theId = 1;
removeCourse(theId);
```

### Validation in MongoDB

MongoDB is NoSQL, it has no schema, if we try to save invalid data, it will be saved without problems.
So, yeah we need to handle the validation xD

Let's go back to the schema we defined:

```js
// notice the first property, we changed it from "String" to that object.
const courseSchema = new mongoose.Schema({
  _id: String,
  name: { type: String, required: true },
  author: String,
  tags: [String],
  date: {
    type: Date,
    default: Date.now
  },
  isPublished: Boolean
});
```

Try to save a course without a name, you'll get an exception.
But, we'll get a rejected promise exception. We didn't handle the rejection of any promise.

Promises? where are they?
In all of these async operations xD

In this case, the save() function returns a promise, that is the one we need to handle rejection for.

```js
async function tryCreateCourse() {
  const defenseAgainstDarkArts = new Course({
    // name: 'Defense Against Dark Arts II',
    author: "Severus Snape",
    tags: ["dark arts", "hogwarts"]
  });

  try {
    return (defenseAgainstDarkArtsID = await defenseAgainstDarkArts.save());
  } catch (error) {
    console.log("Error creating course:", error);
  }
}
```

We wrapped the saving in try/catch, this will catch the exception.
The mongoose validation kicks in without trying to save.

Another way:

```js
async function tryCreateCourse() {
  const defenseAgainstDarkArts = new Course({
    // name: 'Defense Against Dark Arts II',
    author: "Severus Snape",
    tags: ["dark arts", "hogwarts"]
  });

  try {
    // this method returns a Promise<void>
    await course.validate();
  } catch (error) {
    console.log("Error creating course:", error);
  }

  return (defenseAgainstDarkArtsID = await defenseAgainstDarkArts.save());
}
```

In that validate() method, you can pass a callback to execute if validation fails.

#### Built-in Validators in _mongoose_

First, what if we want the price to be required ONLY IF the course is published?

```js
const courseSchema = new mongoose.Schema({
  _id: String,
  name: String,
  author: String,
  tags: [String],
  date: {
    type: Date,
    default: Date.now
  },
  isPublished: Boolean,
  price: {
    type: Number,
    required: function() {
      return this.isPublished;
    }
  }
});
```

Built-in validators:

- For strings, we have:
  - minlength: we pass a number
  - maxlength: we pass a number
  - match: we pass a regex
  - enum: [] we pass an array of valid strings, like ['web', 'mobile', 'network'] for example.
- Numbers and Dates:
  - min
  - max

#### Custom Validators

What if we want to enforce a rule that each course should at least have 1 tag?
We can't rely on _required_, because we can pass it this [].

So, we need a custom validator:

```js
const courseSchema = new mongoose.Schema({
  name: String,
  author: String,
  tags: {
    type: [String],
    validate: {
      validator: function(v) {
        return v && v.length > 0;
      },
      message: "Course should have at least 1 tag"
    }
  },
  date: {
    type: Date,
    default: Date.now
  },
  isPublished: Boolean,
  price: {
    type: Number,
    required: function() {
      return this.isPublished;
    }
  }
});
```

#### Async validation

Sometimes, we need that.
And to do that, we do this:

```js
const courseSchema = new mongoose.Schema({
    // rest of the schema

    // Check here
    validate: {
        // first, we add this property
        isAsync: true,
        // then, we add a CALLBACK Argument to the validator function
        // remember this is one of the ways we handle async stuff
      validator: function(v, callback) {
        // the async logic here
        callback(result);
      },
      message: "Course should have at least 1 tag"
    }
  },

  // rest of the schema
});
```

#### Validation errors

We displayed a message for validation errors, but we can make it more verbose:

```js
const courseSchema = new mongoose.Schema({
    // rest of the schema

    // Check here
    validate: {
        isAsync: true,
      validator: function(v, callback) {
        callback(result);
      },
      message: "Course should have at least 1 tag"
    }
  },

  // rest of the schema
});
```
