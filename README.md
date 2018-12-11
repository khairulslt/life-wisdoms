# Javascript 

### Tell me what you know about NodeJS:

A common misconception about NodeJS is that it is single-threaded. __**NodeJS is not single-threaded.**__ 

What it has is a single thread that listens for connections. As a developer, this single thread is all that is exposed to you - and all of the code you write is executed on this single thread. When a connection is received, Node’s listening thread executes your coded event on the same listener thread. This event either does quick, non-CPU intensive work (like returning static content to a client), or long-running I/O bound operations (like reading data from a database). In the case of the former, the listener thread does in fact block for the duration of the request, but the request happens so quickly that the delay is trivial. In the case of the latter, Node uses V8 and libuv (which it is built upon) to delegate the I/O work to a thread from an underlying pool of native C++ threads. The single listening thread kicks off the work to an I/O worker thread with a callback that says “tell me when you’re done” and immediately returns to listening for the next connection. **It is thus plain to see that Node.js is indeed multi-threaded, though this functionality is not directly exposed to the Node developer.**

# Python
