Fetch API:
==========
Tips:
-----
- Anything between //// and //// is code in the language whose name is written beside the first ////.
- This note doesn't explain what Promises are, so go study them then come back if you don't :P
- This notes and the codes inside has NOT BEEN TESTED, I haven't tried using Fetch API yet, so some things might not work.
- Arguments in arrow functions are put between brackets, but if the arrow function takes one argument, we can omit the brackets.
-----------------------------------------------------------------------------------------------------------------------------------------------
What is Fetch API?
------------------
It's an API for getting resources from the network (which is obvious from the name :P )
It provides an interface for manipulating the HTTP pipeline.
It's similar to XMLHttpRequest, but it's better.

fetch() vs jQuery.ajax():
    - fetch(): won't reject on HTTP error, will resolve with OK status set to 'false', only rejects on network failure.
    - fetch(): won't send/receive cookies from the server, so requests are unauthenticated.

Overview:
---------
Fetch provide 'Request' and 'Response' objects.
we make request like this:
//// JS
WindowOrWorkerGlobalScope.fetch()
//// ENDCODE

This method takes one mandatory argument: the path towhatever we're fetching.
This method returns a Promise that resolves to the Response to that request.
We can pass an 'init' options object as a second argument to this method.

When a response is retrieved in an object of Response, there are some methods available
that define what the body content is and how to handle it.

We can create requests and responses using constructor calls of the functions
Request() and Response(), but that's uncommon.
Instead, requests and responses are created as results of other API actions.

When using Fetch we will use a lot of interfaces: Request, Response, Header, Body, along with the global method 'global fetch'.

--------
Headers:
--------
Headers: an interface that allows you to manipulate the HTTP request/response headers.
    it has an associated Headers list, which is initially empty but is filled in a key/value fashion.
    - We can add things to a Headers object's list using the method 'append()'
    - Our ability to modify a Headers object is determined by its property, 'Guard'.

Guard: it's a property in Headers objects that determines if methods like set() and append() can
    modify the Headers's contents i.e. it determines if the Headers object is immutable or not.
    - It can take these values {immutable, request, request-no-cors, response, none}.
    The default value is 'none'
    - When a Request object is created, the guard's value becomes 'request'.
    - Same for Response. 'response'.
    - Request objects have a property called 'mode', a read-only property that determines if cross-origin
        requests lead to valid responses, its values can be {cors, no-cors, same-origin, navigate}.
        If the mode's value is 'no-cors', the Guard's value becomes 'request-no-cores'.
    - If either error() or redirect() method has been invoked, the Guard becomes 'immutable'.

    - If we try to modify a Headers whose Guard is 'immutable', a TypeError is raised.

Examples: Headers:
------------------
//// JS
//creating a Headers object using constructor fall
var myHeaders = new Headers();

//using some methods
myHeaders.append('Content-type', 'text/html');
myHeaders.get('Content-type'); //Output: text/html
//// ENDCODE

--------
Request:
--------
This interface represents a resource request.

Examples: Requests:
------------------
//// JS
//creating a request
const req = new Request('https://www.mozilla.org/favicon.ico');

//some properties
const URL         = req.url;
const method      = req.method;
const credentials = req.credentials;

//fetching this request
//and processing the Promise returned
fetch(req) //remember that fetch is a global method in Fetch API, so we can use it like this
    .then( (response)=> {
        if(response.status === 200)  { //200 OK
            return response.json();
        } else {
            throw new Error('Something went wrong on api server');
        }
    })
    .then( (response)=> {
        console.debug(response);
        //code to process response as json here
    }).catch((error) => {
        console.error(error);
    });
//// ENDCODE

---------
Response:
---------
This object represents a response to a request.

Examples: Response:
-------------------
//// JS jQuery
//here we'll fetch a request, grab an image, and display it in an <img> tag.
//the fetch returns a Response object.
//NOTE: to get an image, we use the .blob method to give the response it's correct MIME type.
//MIME: Multipurpose Internet Mail Extensions, it basically indicates the nature and format of the response.

const image    = $('#my-image');
const imageURL = 'flowers.jpg'; 
fetch(imageURL)
    .then(function(response) {
        return response.blob();
    }).then(function(blob) {
        const objectURL = URL.createObjectURL(blob);
        image.attr('src', objectURL);
    }).catch(function(error) {
        console.error(error);
    });

//we can create responses using a constructor called
const newResponse = new Response();
//but that's seldom used anyway
//// ENDCODE

-----
Body:
-----
It's a class implemented by both Response and Request objects.
It represents the body of each.
It's the class that has methods like:
.json()
.blob()
.text()
.formData()
basically any methods that are used to process the BODY of the request/response.

-----------------------------------------------------------------------------------------------------------------------------------------------
Using Fetch API:
----------------
a) Simple Fetch request:
//// JS
fetch('http://example.com/movies.json') //fetch is a global method in Fetch API, we call it and give it the URL of the request, the request is made.
    .then(function(response) {          //once the request ends it returns the response as a Response object, this function executes on it and returns a Promise.
        return response.json();         //inside here we just get the json of the response.
    })                                  //this function returns the json inside a Promise
    .then(function(myJson) {            //the same happens to the second Promise
        console.log(JSON.stringify(myJson));    //and so on, we keep making response processing steps in these .then() functions, passing a callback function.
    })
    .catch(function(error) {            //if the request fails, rejection happens and this function executes in case of an error.
        alert(error);                   //we log the error out.
    });
//// ENDCODE

b) POST request, posting json data:
//// JS
var url  = 'http://example.com/movies.json';
var data = {username: 'Moamen'};

fetch(url, {
    method: 'POST',
    body: JSON.stringify(data);
    headers: {
        'Content-Type': 'application/json'
        }
    })
    .then(function(response) {
        return response.json();
    })
    .then(function(reponse) {
        console.log('Success:', JSON.stringify(response));
    })
    .catch(function(error) {
        console.log('Error:', error);
    });
//// ENDCODE

c) PUT request, uploadign a file:

To get a file, we need an object called FormData.
FormData: an interface that provides an easy way to make key/value pairs representing form fields and their values.
    These are used with <form> tags.
    FormData can be looped over using for...of.
    
    Check it out here: https://developer.mozilla.org/en-US/docs/Web/API/FormData

     

//// JS jQuery
var formData  = new FormData();
var fileField = $('#my-input-form');

formData.append('username', 'Moamen');
formData.append('avatar', fileField.files[0]);

var url = 'http://example.com/profile/avatar';

fetch(url, {
    method: 'PUT',
    body: formData
})
.then(response => response.json())
.catch(error => console.error('Error:', error))
.then(response => console.log('Success:', JSON.stringify(response)));
//// ENDCODE

d) Reading a text file line by line:
The chunks read from a response are not broken at linefeeds, they're Uint8Arrays, not strings.
So you have to handle this in order to process line-by-line.
//// JS
async function* makeTextFileLineIterator(fileURL) {
  const utf8Decoder = new TextDecoder("utf-8");
  let response = await fetch(fileURL);
  let reader = response.body.getReader();
  let {value: chunk, done: readerDone} = await reader.read();
  chunk = chunk ? utf8Decoder.decode(chunk) : "";

  let re = /\n|\r|\r\n/gm;
  let startIndex = 0;
  let result;

  for (;;) {
    let result = re.exec(chunk);
    if (!result) {
      if (readerDone) {
        break;
      }
      let remainder = chunk.substr(startIndex);
      ({value: chunk, done: readerDone} = await reader.read());
      chunk = remainder + (chunk ? utf8Decoder.decode(chunk) : "");
      startIndex = re.lastIndex = 0;
      continue;
    }
    yield chunk.substring(startIndex, result.index);
    startIndex = re.lastIndex;
  }
  if (startIndex < chunk.length) {
    // last line didn't end in a newline char
    yield chunk.substr(startIndex);
  }
}

for await (let line of makeTextFileLineIterator(urlOfFile)) {
  processLine(line);
}
//// ENDCODE

3) Checking that fetch was successful:
A fetch() Promise will reject with a TypeError if a network error occurred.
But a response failing doesn't fail the Promise, instead, a property 'Response.ok' is set to false.
So, you need to check on that property.
//// JS
fetch('flowers.jpg')
.then(function(response) {
    if(response.ok) {
        return response.blob(); //blob() is used to get images from a response.
    }
    throw new Error('Error:', response.status, response.statusText);
})  //status is the error number i.e. 404, 403, etc., and the statusText is the text for that error i.e. 'page not found', 'forbidden', etc.
.then(function(myBlob) {
    //do whatever you want with the image here
})
.catch(error => console.log(error));
//// ENDCODE