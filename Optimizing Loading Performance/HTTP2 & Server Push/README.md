# HTTP/2 & Server Push
## Problems with HTTP/1
Have a look at the following diagram on how HTTP/1 generally works.

[HTTP1]

Looks pretty normal and routine right? Well, this malignant design of HTTP/1 actually causes huge performance blocks that can be easily avoided. The problem is that all the requests happen one after the other, and each request requires a separate TCP connection to be opened. These extra round trips and the extra overhead for opening and closing new TCP connections occurs for no real reason, and add up to overall latency. The browser sits mostly idle, except for when it makes requests, while it waits for the server to send everything it needs. This process adds up in the website’s loading time.

## Solution: HTTP/2
HTTP/2 aims to load as much as possible in as few round trips as possible. It also does so over one single TCP connection, which reduces connection establishment and termination overhead. Additionally, it uses HPACK header compression, which also provides a performance boost.

### Server push
HTTP/2 also uses ‘server push,’ which means that in certain cases, the server can send the files that the browser definitely needs to render the page without the browser having to request them.

For instance, suppose a website requires an HTML file, a JavaScript file, a CSS file, and a few other assets (JSON objects and images) to fully render. With HTTP/1, the browser would receive the HTML file, parse it, then make subsequent requests for the JavaScript and CSS files and the other assets, and then finally render.

However, it’s obvious to the developers of the website that the website cannot be rendered without the JS and CSS files. So why make the browser explicitly request them? “Server push” solves this problem by getting the server to send all the necessary files in one round trip over one TCP connection without the client even firing off a request. This provides a significant performance boost.

[HTTP2]

### Multiplexing
Another benefit of HTTP/2 is that multiple files can be sent, or ‘multiplexed,’ in any order over one TCP connection from the server to the client. HTTP/1.1 had ‘pipelining,’ which meant that files could be sent over one connection, but they had to be sent in the correct order. So, if the server did not have file A ready to be sent, it could not just send files B and C first in HTTP/1.1 pipelining. This ordering would block loading, which does not happen with HTTP/2.

Most websites have adopted HTTP/2, and the migration is worth it. Also, HTTP/3 is being developed as of the writing of this course! We’ll save the details of that for when it’s officially adopted.
