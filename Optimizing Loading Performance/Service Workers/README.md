# Service Workers
## Hybrid apps
Hybrid apps are in-between native apps and regular websites, or ‘web apps.’ They use the same technologies as a web app (HTML/CSS/JS), but they adapt to platforms via Xamarin or Cordova. These ‘wrappers’ give them access to native device features, like offline storage. They are available for installation on the app store/play store.

Hybrid apps are great because they significantly save development time. You just develop them once and they are available on every platform. The downside to this is that they can be slow.


## Progressive web apps
Progressive web apps are just websites that work like native apps. In other words, the website will be running in a browser tab, but it’ll feel just like a native device app. Service workers are incredibly essential to create smooth progressive web apps.

Why progressive web apps? Well, for one they are reachable by any device and any browser. They are platform independent. So, PWAs allow for that same smooth and reliable native app experience, but on the browser. There is also an incentive for users to not download native apps: they won’t have to switch to the play store/app store and they won’t have to wait for a big and heavy app to download that will take up space on their phone and will constantly need updates.

The Forbes and AliExpress websites are PWAs. AliExpress has gotten incredible results for making the switch from insisting users download their mobile app to a progressive web app.


## What is a web worker?
Web workers are JavaScript files that run independently of the website off of the main thread of the app. Any heavy non-interactive calculation or anything else that can run in parallel to the site can be offloaded to a web worker. They’re created using the Web Workers API. These web workers are ‘registered’ in the main JavaScript file based on the given API.


## What is a service worker?
A service worker is a specific type of web worker that acts as a proxy between the browser and the server. It also acts as a proxy between the browser and the cache. So, service workers act as a caching agent, and can store content for offline use.

They also give you more control over network requests and allow you to handle push-messaging, too. Since service workers are web workers, they run independently of your app, meaning that they can run even when the app is not open. Progressive web apps use service workers, and can thus work offline or on very slow networks.

Service workers are based on JavaScript promises They also use JavaScript’s fetch and cache APIs.

The following slides show where service workers are in the network architecture.

[Server-client connection with a service worker](./f.jpg)
