# Optimize JavaScript Animations
JavaScript often triggers visual changes. Sometimes that happens directly through style manipulations, and sometimes it’s through calculations, like searching or sorting data. Badly-timed or long-running JavaScript is a common cause of performance issues. Look to minimize its impact where you can.

JavaScript performance profiling can be something of an art because the JavaScript you write is nothing like the code that is actually executed. Modern browsers use JIT compilers and all manner of optimizations and tricks to try and give you the fastest possible execution, which substantially changes the dynamics of the code.

With all that said, however, there are some things that you can do to help your apps execute JavaScript well.


## Use requestAnimationFrame() for visual changes
When visual changes are happening on-screen, you want to do your work at the right time for the browser, which is right at the start of the frame. The only way to guarantee that your JavaScript will run at the start of a frame is to use requestAnimationFrame.

```javascript
/**
 * If run as a requestAnimationFrame callback, this
 * will be run at the start of the frame.
 */
function updateScreen(time) {
  // Make visual updates here.
}

requestAnimationFrame(updateScreen);
```

There are a few of the benefits of using requestAnimationFrame:

- Less CPU consumption
- nimations in inactive tabs stop
- Saves battery
Frameworks or samples may use setTimeout() or setInterval() to do visual changes like animations, but the problem with this is that the callback will run at some point in the frame, possibly right at the end, and that can often have the effect of causing us to miss a frame, resulting in jank.


[Missed frame due to setTimeout()]

### Animation example using setTimeout() vs requestAnimationFrame()
The best way to learn is to learn by example. Here’s an animation example using setTimeout() and requestAnimationFrame(). Notice that the setTimeout() animation is considerably slower because it misses frames. We’ve also purposely added a ‘heavy task’ called heavy_process(), which mimics the possibility that the browser may be running with another JavaScript task on the side.

- index.html
```html
<!DOCTYPE html>
<html>

<head>
  <meta charset="utf-8">
  <title> Animation with RequestAnimationFrame() vs setTimeout() </title>
  <meta name="Educative" content="">
  <meta name="Sample app created for Educative.io" content="">
  <meta name="viewport" content="width=device-width, initial-scale=1">

  <link href="style.css" rel="stylesheet">
  <script src="index.js" defer></script>
</head>

<body>

	<button onclick="start_animation()">Start</button>
	<div id="element"></div>

    setTimeout <span id="time_st"></span>ms<br>
    <div class="box"><div id="st" class="bar"></div></div>
    <br>
    requestAnimationFrame <span id="time_af"></span>ms<br>
    <div class="box"><div id="af" class="bar"></div></div>
</body>

</html>

```
- index.js
```javascript
window.requestAnimFrame = (function() {
    return window.requestAnimationFrame ||
        window.webkitRequestAnimationFrame ||
        window.mozRequestAnimationFrame ||
        window.oRequestAnimationFrame ||
        window.msRequestAnimationFrame ||
        function(callback, element) {
            window.setTimeout(callback, 1000 / 60);
        };
})();

var start_time = (new Date()).getTime();

var st = window.document.getElementById("st");
st.setAttribute("style", "width: 0px;");
var af = document.getElementById("af");
af.setAttribute("style", "width: 0px;");


function heavy_process() {
    var stoptime = 8;
    var start = (new Date()).getTime();
    while ((new Date()).getTime() - start < stoptime) {
        // sleep
    }
}

function render(obj) {
    heavy_process();
    var width = parseInt(obj.style.width) + 1;
    obj.style.width = String(width) + "px";
    if (parseInt(obj.style.width) > 300) {
        document.getElementById("time_" + obj.id).textContent = (new Date()).getTime() - start_time;
        return false;
    }
    return true;
}

function start_animation(){
  (function st_loop() {
      if (!render(st)) {
          return false;
      }
      setTimeout(st_loop, 16.667);
  }());

  (function af_loop() {
      if (!render(af)) {
          return false;
      }
      requestAnimFrame(af_loop);
  }());
}
```

- app.js
```javascript
const express = require('express');
const app = express();
const router = express.Router();

const path = __dirname + '/';
const port = 3000;

router.use(function (req,res,next) {
  console.log('/' + req.method);
  res.header("Access-Control-Allow-Origin", "*");
  next();
});


router.get('/', function(req,res){
  res.header("Access-Control-Allow-Origin", "*");
  res.sendFile(path + 'index.html');
});

app.use(express.static(path));
app.use('/', router);

app.listen(port, function () {
  console.log('Example app listening on port 3000!')
})

```

- style.css
```css
.box {
    width: 300px;
    height: 20px;
    text-align: left;
    background-color: gray;
}

.bar {
    width: 1px;
    height: 100%;
    background-color: #5553FF;
}
```

Also, try switching tabs. You’ll notice that the setTimeout() animation keeps going, whereas the requestAnimationFrame() animation stops where you left it.


## Promoting elements to the GPU
There is a myth that using CSS animations over JavaScript will always result in better performance. Nothing gets recorded in the dev tools’ performance tab because CSS animations (done with transforms) don’t affect document flow. So, browsers can offload them off to the GPU, and the theory is that doing so will give your animations the performance boost they need to run smoothly.

However, not all CSS properties get a GPU boost in animations. Most of the time, transforms (scale, rotation, translation, and skew) and opacity get offloaded to the GPU.

Even if all CSS animations did get onto the GPU, using it has its own overhead. The GPU is a separate computer, much like a server, except the GPU isn’t remote, and data needs to be sent to it and received from it in a certain format. This formatting can incur an overhead that is often overlooked, and can have an impact on performance. For example, offloading to the GPU can lead to a lag in the initial animation startup. This lag also applies to JavaScript-based 3D transforms.

Also, under heavy pressure, CSS transitions are likely to cause synchronization/scheduling issues, possibly due to them being managed in a different thread.

So ‘GPU juicing’ isn’t always a good thing. JavaScript has actually been found to result in better flexibility, improved workflow for complex animations, and richer interactivity. JavaScript animations also often perform just as well or even better than CSS-based animations.
