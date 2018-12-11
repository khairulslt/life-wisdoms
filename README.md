# Javascript 

### Tell me what you know about NodeJS:

A common misconception about NodeJS is that it is single-threaded. 

**_NodeJS is not single-threaded._**

What it has is a single thread that listens for connections. As a developer, this single thread is all that is exposed to you - and all of the code you write is executed on this single thread. 

When a connection is received, Node’s listening thread executes your coded event on the same listener thread. This event either does quick, non-CPU intensive work (like returning static content to a client), or long-running I/O bound operations (like reading data from a database). In the case of the former, the listener thread does in fact block for the duration of the request, but the request happens so quickly that the delay is trivial. In the case of the latter, Node uses V8 and libuv (which it is built upon) to delegate the I/O work to a thread from an underlying pool of native C++ threads. 

The single listening thread kicks off the work to an I/O worker thread with a callback that says “tell me when you’re done” and immediately returns to listening for the next connection. 

**_It is thus plain to see that Node.js is indeed multi-threaded, though this functionality is not directly exposed to the Node developer._**

**_An important note regarding Node.js is that any CPU-intensive code which you write will block the entire system and make your application scale poorly or become entirely unresponsive._** As a result, you would not want to use Node.js when you need to write an application that will do CPU-intensive work such as performing calculations or creating reports.

**_Blocking_** - Blocking refers to operations that block further execution until that operation finishes.

Node is often referred to as a non-blocking I/O model. This means that when requests arrive at a Node server, they are serviced one at a time. However, when the code serviced needs to query the DB for example (long running I/O operation), it sends the callback to a second queue and the main thread will continue running (it doesn't wait). Now when the DB operation completes and returns, the corresponding callback pulled out of the second queue and queued in a third queue where they are pending execution. When the engine gets a chance to execute something else (like when the execution stack is emptied), it picks up a callback from the third queue and executes it.

**_The core idea is that the single listener thread never blocks: it only does fast, cheap processing or delegation of requests to other threads and the serving of responses to clients._**

**_One advantage of non-blocking, asynchronous operations is that it is really fast - and you can maximize the usage of a single CPU as well as memory._**


# Python
