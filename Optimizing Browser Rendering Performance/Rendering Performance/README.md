# Rendering Performance
## Introduction
The Critical Rendering Path is just the steps that browsers take to convert the HTML, CSS, and JavaScript into what a web page looks like. The browser won’t always necessarily touch every part of the pipeline on every frame. Visual changes are made with JavaScript, CSS, or Web Animations. There are three ways the pipeline normally plays out for a given frame when you make a visual change.

## 1. JS > Style > Layout > Paint > Composite
If you change a “layout” property, which is a property that changes an element’s geometry—like its width, height, or its position at the left or top—the browser will have to check all the other elements and “reflow” the page. Any affected areas will need to be repainted, and the final painted elements will need to be composited back together.

So, to avoid expensive browser reflows, it is best not to make changes that affect the layout of a page.

[1](./1.jpg)

## 2. JS / CSS > Style > Paint > Composite
If you changed a “paint only” property, like a background image, some text color, or shadows—in other words, a property that does not affect the layout of the page—then the browser skips layout, but it will still paint.

[2](./2.jpg)

## 3. JS / CSS > Style > Composite
If you change a property that requires neither layout nor paint, then the browser jumps right to compositing.

This final version is the cheapest and most desirable for high-pressure points in an app’s lifecycle, like animations or scrolling. Another example of this is a website pop-up. A layer is added, but no positions or colors of any element change, so no layout or paint is needed.
[3](./3.jpg)

Performance is the art of avoiding work and making any work you do as efficient as possible. In many cases, it’s about working with the browser, not against it. It’s worth bearing in mind that the work listed above in the pipeline differs in terms of computational cost. Some tasks are more expensive than others!
