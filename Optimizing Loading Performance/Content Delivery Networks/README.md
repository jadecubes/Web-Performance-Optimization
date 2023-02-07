# Content Delivery Networks
## The problem
Suppose you run a website, and its hosting server is in San Francisco. When a user from Sydney tries to access your website, however, they will experience some latency due to the large geographical distance between the two locations. This can result in a subpar user-experience, and hence user-dissatisfaction.

Secondly, an attacker can easily compromise your website by launching a DDOS attack against the server in San Francisco, since it’s a single point of failure.

## The solution: content delivery networks
A content delivery network is a network of servers in addition to the main server. They are replica clusters of the main server and they are meant to quickly serve content.

In other words, a CDN puts your content in many places at once, serving the users based on their proximity to whichever server provides them with superior coverage. Each of these servers is usually part of a larger data center.

There can be as many of these as you’d like, and they can be in any location you want. So, if your main user-base is in the United States, Australia, and England, you can have one for each of those countries, or even many. It all depends on your website’s unique needs and what you can afford. Each location is called a Point of Presence, or POP.

These servers also function as an interface between the main server and the end-users. So, no one can DDoS the main server and take it down. There is no longer a single point of failure. Multiple servers also help in masking some outages due to natural disasters. So, if a server gets destroyed due to a flood or fire, there is some backup, and the website won’t go down.

Also, an organization like Netflix may develop their own CDN, while others might use third party CDNs. If an organization has a big enough footprint and/or enough customized needs for a CDN, it may make sense to deploy one’s own CDN.

Let’s go back to our pizza delivery analogy to better understand CDNs. Without a CDN, it’s like one branch of the restaurant exists and it’s serving all of its customers all across a big city. With a CDN, it’s like allowing others to open franchise branches. The main branch still has control of important factors, such as where the supplies come from, but it is offloaded because the franchise branches deliver to the places near them. Take a look at the following slides to better understand this structure:

[CDN](./cdn)

## Do you need a CDN?
If any of the following is true for your website, you should definitely opt for a CDN:

Large amounts of traffic
- A scattered, international user-base
- Expected growth
- Lots of media content, such as images and videos
Otherwise, you won’t notice a difference in speed, but you will still get the benefit of DDoS protection.
