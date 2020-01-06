# Creating a Node.js server

Refer to the github repository: [A simple server written in Node.js](https://github.com/thoughtworks-jumpstart/simple-node-server/blob/master/index.js)

## How does Node.js work

You might be wondering, so how does Node.js allow me to create a website?

You might have created a static HTML website before and have had your static files served by a web server (Apache, for example) so that the browser can view the static files. Navigating to http://localhost/index.html will allow you to view the index.html page.

For Node, you will have to write the web server yourself. Node provides you with the framework to write the web server. The app you write is the web server. Don't worry, even though it sounds daunting, the frameworks have made it easy for you to write a web server, plus the control you gain over your application is worth it.

## Creating the server

You will require a built-in module of Node.js `http` which allows Node.js to transfer data over the Hyper Text Transfer Protocol (HTTP). Import this into the file.

Note that this does not use _https_ by default (a security issue!). You can decide to use HTTPS later.

```js
// index.js
const http = require("http");
const PORT = 3000;

const server = http.createServer();
```

We shall later make the server listen to port 3000.

## Responding to requests

We want the server to respond to a request when a request comes in. You can add a function that is called every time a HTTP request is made to the server. It is called the handler function.
It takes two arguments, `req` is an object that represents the request and `res` is an object that represents the response.

```js
// index.js
server.on('request', (req, res) => {
console.log('Method', req.method);
console.log('Path', req.url);
```

### Request object

It is an instance of _http.IncomingMessage_. See [w3schools.com HTTP Incoming Message](https://www.w3schools.com/nodejs/obj_http_incomingmessage.asp) for more info.

### Response object

It is an instance of _http.ServerResponse_. See [w3schools.com HTTP Server Response](https://www.w3schools.com/nodejs/obj_http_serverresponse.asp) for more info.

### Event emitter

The `request` event is an example of an event emitter of Node.js.
To know why Node.js use event emitters with the HTTP server, [read more here](https://codeburst.io/event-emitters-and-listeners-in-javascript-9cf0c639fd63).

## Modifying the response

Set the content-type header to `text/plain` because our response will be in plain text and set status code to 200:

```js
// index.js
res.writeHead(200, { "Content-Type": "text/plain" });
```

If a user accesses the url `/books` (a GET request is made), display "Here are the books 📖" to the user:

```js
// index.js
if (req.url === "/books" && req.method === "GET") {
  res.end("Here are the books 📖");
}
```

Allow a user to create a new book when a POST request is made to `/books`:

```js
// index.js
else if (req.url === '/books' && req.method === 'POST') {
res.end('You have created a new book');
}
```

Display a default message if other urls are accessed

```js
// index.js
else {
res.end('Thank you for visiting the server');
}
})
```

## Listening to port

To serve requests, the server needs to listen to a specific port number. Listen to `PORT` number that was set earlier:

```js
// index.js
server.listen(PORT, () => {
  console.log("Server is now listening on PORT", PORT);
});
```

You could also use a shorthand for adding the handler function:

```js
// index.js
const server = http.createServer((req, res) => {
  // put the above request handling code here
});
```

## Running the server

Run the server:

```
node index.js
```

Try visiting http://localhost:3000 and you will see "Thank you for visiting the server".
Try other URLs like http://localhost:3000/hello or http://localhost:3000/bye. Does the console output change? Does the page looks the same or different?

## Making requests to the server

Now try visiting http://localhost:3000/books.

To make a POST request to http://localhost:3000/books, try using a API tester tool like Postman.

You could also make a POST request using cURL:

```
curl -X POST http://localhost:3000/books
```

To shut down the server, press Control+C.

To know more details, check the official Node.js docs about the anatomy of an [HTTP transaction](https://nodejs.org/es/docs/guides/anatomy-of-an-http-transaction/).

## Creating Node.js servers using Express.js

Node.js is a low-level, I/O framework. It does not come with security, user sessions, error handling etc. This is where the Express.js framework comes in. Express.js is built on top of Node.js to make the creation of Node.js servers a lot simpler. We will discover this when we create the same simple server in Express.js.
