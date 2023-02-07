# Progressive Rendering
## Progressive rendering
You might have noticed that most modern websites render gradually. They don’t appear all at once, but in parts. This is because the page rendering process is not done all in one go. The browser starts receiving, parsing, and rendering whatever HTML it can, even if it has not received all of it.

This is done to make waiting a little easier for the user. Try counting how long it takes for a website to fully render and imagine if you had to wait that long to see anything on the screen!

This method is called progressive rendering, and is sort of in-between server-side and client-side rendering. The components of the page are rendered on the server and sent in order of priority. So, the highest priority components are rendered on the server, sent to the browser, and painted on the browser first. This gives the effect that you see on most modern websites, where parts of it are loaded and displayed first.

One way that content can be prioritized is by prioritizing what’s visible. One popular method to do this for images is called ‘lazy loading.’ Here, an image is only loaded onto the page when the user scrolls to it and it becomes visible. There are many ways to do this. One way is to use the HTML lazy attribute.
```html
<img src="myimage.jpg" loading="lazy" alt="..." />
```

## Example
### Progressive rendering
[Example](./progressive)
### Server-side rendering
[Example](./ssr)
