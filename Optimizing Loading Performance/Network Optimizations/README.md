# Network Optimizations
## Reduce time to first byte
In addition to the amount of time it takes for your page to fully load, you’ll also want to take a look at the amount of time it takes to start loading.

Time to first byte, or TTFB, is the amount of time a browser has to wait before getting its first byte of data from the server. Google recommends a TTFB of less than 200 ms.

Here’s a snapshot of the ‘timings’ tab that gets shown when a response file on the network tab of devtools is clicked. It shows a website that has an excessive TTFB.

[Time to first byte in inspector tool]

In general, most issues with slow TTFB are caused by network issues, dynamic content creation, DNS lookup time, or traffic. We have control over two of these factors: dynamic content creation and DNS lookup time.

### Speed up DNS resolution
The amount of time DNS resolution takes depends on how fast your DNS provider is. If your DNS server is slow, it may be time to switch to a faster DNS provider.

[Speed of popular DNS services in February, 2020, taken from: http://www.solvedns.com/dns-comparison/2020/02]

Take a look at these DNS server speeds from multiple locations across the world. These are popular services that people may choose over their ISP to speed up DNS resolution. You can look at one of these to decide which service to pick.

## Caching
To refresh your memory: caching is the mechanism of storing data that is usually temporarily calculated on a high-speed storage layer called a ‘cache.’ In this case, the ‘data’ would be the website.


### Enable browser caching
Some unchanging parts of a website (pages or parts of pages) can be stored or cached, which means they won’t have to be requested from the server. These unchanging components can be the website’s logo, for instance. The web server can tell the browser to store these files and not download them when you come back for a specified period of time. This saves bandwidth and speeds up the website.

### Enable server caching
How would ‘server-side’ caching work? What would the point of storing files in a cache when they’re already stored on the server be? Well, oftentimes, the same version of the website is not served to all users. Consider the example of an e-commerce website. Each user will have a unique check-out cart and logged-in session. So, for the duration of a particular user’s session, we may enable caching. This will reduce the number of times a call is made to the database, which will speed up the website.

Several other forms of caching are also applicable. The web server caches frequently accessed files in the memory, the database server caches frequently accessed records, the application can cache frequently accessed data values into a cache, and so on.

## Reduce redirects
An HTTP redirect is a way to forward visitors and search engines from one URL to another, resulting in another HTTP request.

Redirects are often necessary and useful to take care of broken links due to moved or deleted pages. But these additional HTTP requests can create an overhead, which can negatively impact performance, so they are best kept to a minimum.

## Serving assets from multiple domains
Usually, serving assets from multiple domains instead of one is recommended because of the performance boost that doing so provides. Here’s how it works:

### Parallelization
Because most browsers have a limit of six connections per domain, a domain with more than six assets to serve would get blocked on parallelization. So, serving some assets from other domains can increase concurrency. Secondly, offloading some assets to other servers will distribute the load, and therefore decrease the time it takes to serve them, too. For example, suppose a server takes 10ms to serve one asset and has to serve 60 assets. It’s going to take 60×10=600 ms to serve them all. Now suppose the domain owner offloads half of these to another server that also takes 10ms to serve one asset. Then, each server would only have to serve 30 assets, which would take each 30×10=300 ms to serve all of the website’s assets in parallel. Offloading to just one other domain halved our loading time in our hypothetical world!

### Reduced header overhead
Servers usually set cookies on browsers. This helps identify the user and customize their experience. The cookie is sent with every request made to the server, but the server does not require a cookie to serve all of the assets. The website’s static logo image, for example, would not need a user to ‘show identification.’ Suppose this cookie is 1 kilobyte. That would mean that for 60 requests, 60 extra kilobytes are sent over the network.

So, offloading all static assets that don’t require user authentication (cookies) can boost page speed.
