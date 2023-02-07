# Parsing & Page Rendering
## What a browser is made of
The browser is made of a few different components, many of which play a critical role in rendering.

- User interface: All the interactive elements, including buttons on a browser, except for the window where the page is displayed.

- Browser engine: The go-between for the UI and the rendering engine.

- Rendering engine: Renders the requested content.

- Networking: Handles network calls, such as HTTP requests, which we studied previously.

- UI backend: Used for drawing basic widgets, such as list boxes and windows. It exposes a generic interface that uses OS methods underneath.

- JavaScript engine: Parses and executes JavaScript code.

- Data storage: The browser has some persistent memory available to store information like cookies, passwords, and history.

## The importance of rendering performance
Web user’s today expect that the pages they visit will be interactive and smooth, and that’s where you need to increasingly focus your time and effort. Pages should not only load quickly, but also run well; scrolling should be stick-to-finger fast, and animations and interactions should be silky smooth.

To write performant sites and apps, you need to understand how HTML, JavaScript, and CSS are handled by the browser, and ensure that the code you write (and the other third-party code you include) runs as efficiently as possible.

## 60fps and device refresh rates
Most devices today refresh their screens sixty times a second. If there’s an animation or transition running, or the user is scrolling the pages, the browser needs to match the device’s refresh rate and put up one new picture, or frame, for each of those screen refreshes.

So, each of those frames has a budget of just over 16ms (1 second / 60 = 16.66ms). In reality, however, the browser has housekeeping work to do, so the frames really have a budget of 10ms. When you fail to meet this budget, the frame rate drops, and the content judders on screen. This is often referred to as jank, which negatively impacts the user’s experience.

## Rendering engines
There are a few different types of rendering engines used by different browsers. Rendering engines display whatever information was requested. While HTML, XML documents, and images are all supported by default in all browsers, other files like PDFs can be rendered via plugins and extensions added onto a browser.

## HTML parsing
As the rendering engine receives the HTML file, it starts ‘parsing’ it into a ‘parse tree.’ Essentially, all the elements of the HTML file are converted into a ‘tree’ called the DOM.

### DOM
The DOM (Document Object Model) is the ‘object representation’ of the HTML document. The DOM is created before scripts or external factors make any changes to it.

[Sample DOM tree](./dom.jpg)


```html
<html>
 <head>
   <title> Page Title </title>
 </head>
 <body>
   <h1> Level 1 heading! </h1>
   <p id = "Educative">
     Front-end programming is fun!
   </p>
 </body>
</html>
```
## JavaScript
Typically, JavaScript is used to handle work that will result in visual changes, whether it’s jQuery’s animate function, sorting a data set, or adding DOM elements to the page. It doesn’t have to be JavaScript that triggers a visual change, though: CSS Animations, Transitions, and the Web Animation APIs are also commonly used.

## Render tree construction / style calculations
A ‘Frame Tree,’ or ‘Render Tree,’ is created by traversing the DOM nodes and calculating the CSS style values for each node. So, styling information and HTML together are used to create the render tree.

The render tree is made up of rectangles with visual cues, like color and dimension, in the order that they are to be displayed in.

## Layout
Next, the exact coordinates of each element that each node represents are calculated. This step is called the layout. Since the size and placing of each element affects the rest, and the size of the children of all the elements depend on their respective parent elements, there is quite a lot to calculate for the browser here.

For example, the width of the <body> element typically affects its children’s widths, and so on, all the way up and down the tree.

## Painting
Finally, these elements have to be converted into actual pixels on the screen. This is done in two parts:

1. Creating a list of draw calls.
2. Filling in the pixels, or ‘rasterizing.’
Whenever you see paint records in DevTools, you should think of them as including rasterization. In some architectures, creating the list of draw calls and rasterizing are done in different threads, but that isn’t something under the developer’s control.

The drawing is typically done onto multiple surfaces, often called layers. So now, the ‘render tree’ is finally part of a displayed webpage.

## Compositing
Since parts of the page were drawn onto potentially multiple layers, they need to be drawn on the screen in the correct order so that the page renders correctly. This is especially important for elements that overlap, since a mistake could result in one element appearing over the top of another incorrectly. Also, note that compositing takes advantage of the GPU!

[Key points in the pixels-to-screen pipeline](./compositing.jpg)

## Jank
Any and all of these steps may introduce jank (page scrolling or tapping on touchscreens that produce lag in the response time). So, learning how your code influences each of these will help immensely with optimizing performance.
