# Analyzing Render Performance with Developer Tools
## Data
In the last lesson, we opened up developer tools, recorded an animation, and got some data. What we ended up seeing is something like this. We’ll break this down into digestible chunks and explain everything step-by-step in this lesson.

[The results of the profile]

We’ll start from the top chart and work our way to the last one.

The x-axis for all of these represents time. Notice the very first bar that appears at the top in the screenshot above. It shows the time in milliseconds of your recording. All the panels capture data about a certain point in time, including screenshots of the app/website at that time. This is sort of like a CCTV video of your app, and you’re like a detective trying to find clues!


## FPS / CPU / NET

[The FPS chart, outlined in blue]

### Frames per second
Look at the FPS chart. Whenever you see a red bar above FPS, it means that the framerate dropped so low that it’s probably harming the user experience. In general, the higher the green bar, the higher the FPS.

### CPU
Below the FPS chart, you see the CPU chart. The colors in the CPU chart correspond to the colors in the Summary tab, at the bottom of the Performance panel. The fact that the CPU chart is full of color means that the CPU was maxed out during the recording. Whenever you see the CPU maxed out for long periods, it’s a cue to find ways to do less work.

We’ll ignore the NET panel because it looks at network requests, which are irrelevant to this lesson.


### Hovering
You’ll notice that hovering over these panels shows a screenshot of the app at a specific point in time.



### Clicking / scrubbing
You can click on any part of these panels. You’ll notice the other panels update to reflect what exactly was going on at the point in time that you clicked them. You can click and drag to expand the window. You can then use the ‘w, a, s, d’ keys to drag the window around. This is called scrubbing, and it’s useful for manually analyzing the progression of animations. Experiment with this.

## Frames
In the Frames panel, hover your mouse over one of the green squares. DevTools shows you the FPS for that particular frame. Each frame is probably well below the target of 60 FPS. You can also click on it to expand it and see each frame.

[Hovering over a frame. The FPS is 15, which is well below the target of 60.]

Of course, with this demo, it’s pretty obvious that the page is not performing well. But in real scenarios, it may not be so clear, so having all of these tools to make measurements comes in handy.
## Finding the bottleneck
Now that you’ve measured and verified that the animation is not performing well, the next question to answer is: why?

Note the summary tab. When no events are selected, this tab shows you a breakdown of activity. The page spent most of its time rendering. Since performance is the art of doing less work, your goal is to reduce the amount of time spent rendering.

[The Summary tab, outlined in blue]

Expand the Main section and select a frame from the FPS/CPU/NET chart. Try to zoom in on a single Animation Frame Fired event. DevTools will show you a flame chart of activity on the main thread over time. The x-axis represents the recording over time. Each bar represents an event. A wider bar means that an event took longer. The y-axis represents the call stack. When you see events stacked on top of each other, it means the upper events caused the lower events.

[The Main section, outlined in blue]

Note the red triangle in the top-right of the Animation Frame Fired event that we’ve circled in blue. Whenever you see a red triangle, it’s a warning that there may be an issue related to this event. The Animation Frame Fired event occurs whenever a JavaScript requestAnimationFrame() callback is executed.

Click the Animation Frame Fired event. The Summary tab now shows you information about that event. Note the reveal link. Clicking this link causes DevTools to highlight the event that initiated the Animation Frame Fired event. Also note the index.js:95 link. Clicking this link jumps you to the relevant line in the source code.


[More information about the Animation Frame Fired event]

Under the app.update event, there’s a bunch of purple events. If they were wider, it looks as though each one might have a red triangle on it. Click on one of the purple Layout events now. DevTools provides more information about the event in the Summary tab. Indeed, there’s a warning about forced reflows (another word for layout).

In the Summary tab, click the index.js:71 link under Layout Forced. DevTools takes you to the line of code that forces the layout.


[The line of code that caused the forced layout]

The problem with this code is that, in each animation frame, it changes the style for each square, and then queries the position of each square on the page. Because the styles changed, the browser doesn’t know if each square’s position changed, so it has to re-layout the square in order to compute its position.

Phew! That was a lot to take in, but you now have a solid foundation in the basic workflow for analyzing runtime performance. Good job.


## Exercise: analyze or ‘profile’ your favorite website!
