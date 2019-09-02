Eloquent JS notes part 3:
=========================
Tips:
-----
- anything between //// and //// is code.
- notes will be divided according to projects in the book, so:
    - part 1: chapters 1 until 6. (Done)
    - part 2: chapters 8 until 11.
    - part 3: chapters 13, 14, 15 (we read these before).
    - part 4: chapters 17 until 20.
- chapters 7, 12, 16, 21 are projects, I'm not sure if there will be time to do them honestly so I don't know if I'll include them :'D
- I assume that you're not a noob at programming so some stuff is skipped e.g. what are loops, etc., not that being a noob is bad or anything, we've all been noobs xD
-------------------------------------------------------------------------------------------------------------------------------
Chapter 18: HTTP and forms:
---------------------------
When you open a link in a browser e.g. eloquentjavascript.net/18_http.html
    - browser looks up address of server associated "eloquentjavascript.net"
    - browser opens TCP connection to it on port 80/
    - If server accepts connection, browser sends a GET (request):
        GET /18_http.html HTTP/1.1
        Host: eloquentjavascript.net
        User-Agent: <the browser's name>
    - Server responds by a (Response):
        HTTP/1.1 200 OK
        Content-Length: 65585
        Content-Type: text/html
        Last-Modified: <date>

        <!doctype html>
        ... the rest of the document.

GET: the method of the request, it requests to get something.
    DELETE, PUT, POST: other methods of request.

Request format:
    <method> <path of resource> HTTP/<version of http>

Response format:
    HTTP/<version> <response code> <response string>
    name: value <these are some info about the response, they can be in the request too>
    <body of response/request, if there is>
Response code: a code that represents a certain response 
Response string: a human-readable response.
e.g.:
    200 OK: means the request is accepted.
    404 NOT FOUND: means that the resource you're trying to get is NOT found. (That's what "404 not found" is).

