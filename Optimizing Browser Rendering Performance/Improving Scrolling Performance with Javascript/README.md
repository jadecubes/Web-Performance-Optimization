# Improving Scrolling Performance with JavaScript
## Reduce complexity or use web workers
JavaScript runs on the browser’s main thread, right alongside style calculations, layout, and, in many cases, paint. If your JavaScript runs for a long time, it will block these other tasks, potentially causing missed frames. You should be tactical about when JavaScript runs, and for how long. For example, if you’re in an animation like scrolling, you should be looking to keep your JavaScript somewhere in the region of 3-4ms. Any longer than that and you risk taking up too much time. If you’re in an idle period, you can afford to be more relaxed about the time taken.

In many cases, you can move pure computational work to Web Workers if, for example, the work doesn’t require DOM access. Data manipulation or traversal, such as sorting or searching, are often good fits for this model, as are loading and model generation.

```javascript
var dataSortWorker = new Worker("sort-worker.js");
dataSortWorker.postMesssage(dataToSort);

// The main thread is now free to continue working on other things...

dataSortWorker.addEventListener('message', function(evt) {
   var sortedData = evt.data;
   // Update data on screen...
});
```

Not all work can fit this model: Web Workers do not have DOM access. Where your work must be on the main thread, consider a batching approach, where you segment the larger task into micro-tasks, each taking no longer than a few milliseconds, and run the inside of requestAnimationFrame() handlers across each frame.

```javascript
var taskList = breakBigTaskIntoMicroTasks(monsterTaskList);
requestAnimationFrame(processTaskList);

function processTaskList(taskStartTime) {
  var taskFinishTime;

  do {
    // Assume the next task is pushed onto a stack.
    var nextTask = taskList.pop();

    // Process nextTask.
    processTask(nextTask);

    // Go again if there’s enough time to do the next task.
    taskFinishTime = window.performance.now();
  } while (taskFinishTime - taskStartTime < 3);

  if (taskList.length > 0)
    requestAnimationFrame(processTaskList);

}

```
There are UX and UI consequences to this approach, and you will need to ensure that the user knows that a task is being processed, either by using a progress or activity indicator. In any case, this approach will keep your app’s main thread free, helping it stay responsive to user interactions.

## Know your JavaScript’s “frame tax”
When assessing a framework, library, or your own code, it’s important to assess how much it costs to run the JavaScript code on a frame-by-frame basis. This is especially important when doing performance-critical animation work, like transitioning or scrolling.

The Performance panel of Chrome DevTools is the best way to measure your JavaScript’s cost. Typically, you get low-level records, like this:

[A frame chart of JavaScript calls in the Chrome developer tools' performance tab](./chart.jpg)

The Main section provides a flame chart of JavaScript calls, so you can analyze exactly which functions were called and how long each took.

Armed with this information, you can assess the performance impact of the JavaScript on your application, and begin to find and fix any hotspots where functions are taking too long to execute. As mentioned earlier, you should seek to either remove long-running JavaScript, or, if that’s not possible, move it to a Web Worker, freeing up the main thread to continue on with other tasks.

## Avoid micro-optimizing your JavaScript
It may be cool to know that the browser can execute one version of a function 100 times faster than another version of the function, but it’s almost always true that you’ll only be calling functions like these a small number of times per frame, so it’s normally wasted effort to focus on this aspect of JavaScript’s performance. You’ll typically only save fractions of milliseconds.

If you’re making a game or a computationally expensive application, then that is likely an exception to this guidance, as you’ll typically be fitting a lot of computation into a single frame. In that case, everything helps.

In short, you should be very wary of micro-optimizations because they won’t typically map to the kind of application you’re building.
