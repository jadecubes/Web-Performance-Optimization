# Client-side Rendering vs. Server-side Rendering
## Client-side rendering
A website that uses client-side rendering will have the server send a mostly empty HTML file to begin with. It will have links to CSS in the <head>. It will also have links to JavaScript in the <script> just before the closing </body> tag. The <body> will otherwise be empty. This is to stop JavaScript loading from blocking content loading. So, the content is loaded and the scripts are loaded next.

An alternative to this is to use async in the script tag, like so:
```html
<script src=”main.js” async></script> 
```
The async attribute tells the browser to load the file asynchronously. This means it is loaded as a background task, so it will not block the main thread and hence will not block the content from loading.

While we’re on the topic, it’s recommended to put links to CSS style sheets in <head></head>, too. This is so that the browser styles the HTML as it loads. If style sheets are put at the bottom of the HTML, the browser will have to restyle and render the whole page from the top, which can cause a performance bottleneck.

However, unless the script is needed before the content for some reason, it’s best to keep it at the end of the body. Here’s how client-side rendering works:

[CSR](./csr)

Also, note that in-lining styles like this:
```html
<p style="font-size: 40px; color: blue;">Interviews can be fun!</p>
```
is not a good idea, because you’ll end up duplicating a bunch of styles, negatively impacting render time, and ultimately, you won’t harness the power of full CSS features (such as classes).

### Pros
JavaScript bundles can be cached to speed things up in the future.

### Cons
- Loading content may take a while because requests have to travel all the way to the server, which can be very far away.
- SEO takes a hit because all search engines see is an empty HTML file.

## Server-side rendering
In server-side rendering, the whole web page is compiled on the server. The HTML is completely populated with the content, which is sent to the client. Next.js and Gatsby use this technique. Here’s how server-side rendering generally goes:

[SSR](./ssr)

### Pros
Search engines will be able to crawl the site, resulting in better SEO because the pages will be populated with content.
### Cons
A page will have to be rendered on the server and reloaded every time a new page on the site is visited, which will lead to full page reloads.
The server will receive frequent requests, which can easily lead to the server getting flooded with requests and slowing down.
