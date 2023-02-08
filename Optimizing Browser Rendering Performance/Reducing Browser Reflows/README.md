# Reducing Browser Reflows
Reflow is the name of the web browser process for re-calculating the positions and geometries of elements in the document for the purpose of re-rendering part or all of the document. Because reflow is a user-blocking operation in the browser, it is useful for developers to understand how to improve reflow time and to understand the effects of various document properties (DOM depth, CSS rule efficiency, different types of style changes) on reflow time.

Sometimes, reflowing a single element in the document may require reflowing its parent elements and any elements which follow it.

There are a great variety of user actions and possible JavaScript/CSS/HTML (DHTML) changes that can trigger reflow. Resizing the browser window, using JavaScript methods involving computed styles, adding or removing elements from the DOM, and changing an element’s classes are a few of the things that can trigger reflow.

It’s also worth noting that some operations may cause more reflow time than you might imagine. Not all changes to the style in JavaScript cause a reflow in all browsers, and the time it takes to reflow varies from browser to browser. Modern browsers are getting better at decreasing reflow times.

## What causes reflow?
Here are a few reflow causes:

- Window resizing
- Changing the font of an element
- Making any changes to the content itself
- Adding or removing a style sheet
- Adding or removing classes
- Modifying size
- Calculating size
- Changing position
- Calculating position
- Changing orientation
- DOM manipulation

## How to avoid reflow
Here are some easy guidelines to help you minimize reflow in your web pages:


### DOM
- Reduce unnecessary DOM depth. Changes at one level in the DOM tree can cause changes at every level of the tree, all the way up to the root, and all the way down to the children of the modified node. This leads to more time being spent performing reflow.

- That being said, any class change should be done at the lowest level of the DOM tree. This is because all the elements under a certain element will undergo reflow when that element is changed.

- Batch DOM manipulations. Instead of doing them one at a time, do them in batches. A lot of front-end frameworks provide this very advantage.
## JavaScript
If you make complex rendering changes, such as animations, do so out of the flow. Use position-absolute or position-fixed to accomplish this.

## CSS
- Minimize CSS rules, and remove unused CSS rules.

- Avoid unnecessary, complex CSS selectors, descendant selectors in particular, which require more CPU power to do selector matching.

- Try not to modify inline styles too much.
## Promoting elements to the GPU
There is a myth that using CSS animations over JavaScript will always result in better performing animations. The theory is that CSS animations use the GPU, and that doing so will give your animations the performance boost they need to run smoothly.

However, not all CSS properties get a GPU boost in animations. Most of the time, transformations (scale, rotation, translation, and skew) and opacity get offloaded to the GPU.
