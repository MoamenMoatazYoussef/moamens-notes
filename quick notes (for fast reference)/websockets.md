WebSocket:
==========
Sockets: it's like a tunnel between two computers, unlike normal HTTP requests and Ajax, sockets are connections that are kept running, or "live" as long as we want i.e. they keep a TCP/UDP layer alive as long as both devices are connected.

These are useful for continuous stream stuff, like video streaming, etc.

JS uses built-in web sockets, so you don't need to import stuff or whatever.
``` js
// the string argument isn't constant, it can be changed, 
// this goes to websocket.org which can be use for testing websockets
var sock = new WebSocket("ws://echo.websocket.org");
```
Chrome dev tools support webSocketmonitoring too, go to Network tab and filder on WS instead of XHR or All.

You can go to websocket.org and use it to try websockets.

Getting started with WebSockets:
- Create a basic HTML file and in it a script tag.
- Inside the script tag:
``` js
var sock = new WebSocket(ws://echo.websocket.org);
sock.onopen = event => console.log(event);
```
- Open the page, see the log output, you'll find an Event object with a lot of stuff.
- Update the  js:
``` js
var sock = new WebSocket(ws://echo.websocket.org);
sock.onopen = event => {
	alert('Connected successfully');
};

setTimeout(() => {
	sock.send("Hi guys");
}, 1000);

sock.onmessage = event => {
	console.log(event);
};
```
- Reload the page, you should see an alert after 1 second, and in the Log output you'll see the event.
- In the network tab (filter to ws), you will see the "Hi guys" message we typed.

- Change the js. console.log(event.data) instead of console.log(event).
- Refresh the page, you'll get "Hi guys" in the log output :)

WebSockets with Nodejs:
- Create a folder and cd into it.
- >> npm init
- >> npm install ws
- Create index.js
``` js
var Server = require("ws").Server;
var server = new Server({ port: 5001 });

const handleMessageReceived = message => {
	console.log("Message received:", message);
}

const handleConnection = () => {
	ws.on("message", handleMessageReceived
}

server.on('connection', handleConnection);
```
- >> node index.js
- Remember the index.html we used earlier? get it.
- Change the string argument in WebSocket to ws://localhost:5001
- Run the page, you should get a Hi guys message in the cmd running the SERVER.

Replying to the client:
- Update the js
``` js
var Server = require("ws").Server;
var server = new Server({ port: 5001 });

const handleMessageReceived = message => {
	console.log("Message received:", message);
	if(message === "Hi guys") {
		ws.send("Hey there");
	}
}

const handleConnection = () => {
	ws.on("message", handleMessageReceived
}

server.on('connection', handleConnection);
```
- If you want, create a button in the html page and make its onclick point to a function that runs the message sending code. (and remove the alert)
- Refresh the page, open the chrome dev tools, network tab, filter to ws, and see the RECEIVED messages, you should get the "Hey there" sent from the server :)

A little more work:
- >> npm install nodemon
- >> nodemon index.js
- In index.html, add an <input> tag with an id.
- In the script, 
	- remove the sock.onopen function.
	- modify the button's onclick to get the input's value, then send it using sock.send.
- index.js:
``` js
var Server = require("ws").Server;
var server = new Server({ port: 5001 });

const handleMessageReceived = message => {
	console.log("Message received:", message);
	ws.send("From server:", message);
}

const handleConnection = () => {
	ws.on("message", handleMessageReceived
}

server.on('connection', handleConnection);
```
- Refresh the page, type something and press the button, you should get a message with your input back :)
- Also, add this to the server.on function in the server:
``` js
ws.on("close", () => console.log("I lost a client :(");

console.log("A new client connected");
```
- Duplicate the page tab, see the server, should say twice "A new client connected" :)
- Close one tab, see the server, should say "I lost a client :(" :)
- Try opening many tabs and closing them to see that it handles connection to multiple clients under the hood.

Assigning an ID for each client helps the server know which client disconnected.

And that's it, basics of websockets :)
