# Optimizing Requests
## TCP connections
For a host to request a website, it first needs to establish a connection with the server called a ‘TCP connection.’ This TCP connection requires a ‘handshake,’ much like the TLS handshake we studied that requires at least three back and forth messages between the client and the server. Once a connection is established, the TLS handshake can occur if the connection is to be encrypted, which requires two messages for TLS 1.3.

So, a total of at least five messages need to be exchanged before any requests can be made. This is a considerable overhead, and optimizing it deserves attention. In this lesson, we will study common measures to address this issue.

## One connection per request
If one connection is established per request, we’ll end up having to do multiple handshakes per request. This will take too much overhead and is not feasible.

## One connection per host
If one connection is established per host for the entirety of the exchange, we would completely avoid the overhead of multiple handshakes. This can be achieved with the connection keep-alive header in HTTP/1.1, which instructs browsers to keep a connection open.

Here’s the catch though: the requests go out in order on a single connection. This means that the first request that is made goes out first, while the rest are held back in a queue until the first request’s response is received fully.

Multiple requests are often fired more or less simultaneously in the browser. So, if ten requests are fired at once, we’ll have to wait for a response to the first one to come in before we can even send the second one. Hence, requests would be held back, resulting in a delay if one connection is used per host.


### HTTP pipelining
HTTP/1.1 has another feature called ‘HTTP pipelining,’ which enables the browser to send multiple requests in quick succession without waiting for their responses on a single TCP connection. That fixes our problem. Right?

Actually, no. This feature has a catch, too. The server, in this case, has to respond to the requests in the same order that it has received them.

So, if the server ends up calculating a response for the fifteenth request it receives, it won’t be able to send that response until it has a response for the first one, which again introduces a delay. So, this approach is generally avoided.


## Solution HTTP/1.1: fixed number of parallel connections
A good compromise is to use a fixed number of connections per host. So, if you have six connections for your browser, six requests can be fired off simultaneously, but the seventh will be queued until a connection frees up.

These connections may be kept open for a while in case more requests come in, and can be closed after not being used for a certain period of time to free up memory.

## Solution HTTP/2: multiplexing
With HTTP/2, the constraint that the server needs to send the responses in the order it got the requests is gone. Multiplexing enables mapping the requests and responses.

So, the server can immediately respond with whichever request is processed first, and HTTP/2 will map each response to the original request. Generally, all of this is done through one TCP connection. We’ll study HTTP/2 in a bit more detail in an upcoming lesson.

## Looking at real requests
1. Open up your favorite browser.
2. Right-click anywhere and choose ‘inspect element’ from the drop-down menu.
3. Open up the network tab that you will find on the top bar.
4. Go to your favorite website and see how the requests are made. Try to figure out what approach is being used, and if it can be optimized.

We went to Educative.io. Look at how the Connection ID for all the requests is the same, and how h2, or HTTP/2, is being used. Our website is using multiplexing!

[HTTP/2](h2.jpg)

```
Note you can choose from a collection of very interesting fields in these columns in most browsers (we tried it on Safari, Chrome, and Firefox). Right-click on any column header and choose what looks intriguing to you. Try experimenting with what you find, and see if you can pick up anything insightful.
```
[Options](./try.jpg)
