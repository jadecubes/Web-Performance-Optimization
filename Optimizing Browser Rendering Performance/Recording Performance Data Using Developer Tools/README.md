# Recording Performance Data Using Developer Tools
```
📝 This lesson is based on Chrome Version 85, so there might be slight differences, but the overall process should be similar from browser to browser and version to version.
```

Runtime performance is how your page performs when it is running, as opposed to when it is loading. We’ll load a given app in this lesson and actually get to take a close look at how all the runtime steps, such as JavaScript, styling, and painting happen in the real world.


## Run the example app
We’ll use the following code example to study performance. Click ‘run’ and open up the given ‘https://yourAppURL.educative.run’ link in a new incognito window, as shown below. Incognito Mode ensures that no other tabs or extensions interfere with the website’s performance.

[Open the app in an incognito window](./open.jpg)

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

- index.html
```html
<!--
  Copyright 2016 Google Inc.
  Licensed under the Apache License, Version 2.0 (the "License");
  you may not use this file except in compliance with the License.
  You may obtain a copy of the License at
  http://www.apache.org/licenses/LICENSE-2.0
  Unless required by applicable law or agreed to in writing, software
  distributed under the License is distributed on an "AS IS" BASIS,
  WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND, either express or implied.
  See the License for the specific language governing permissions and
  limitations under the License.
-->

<!-- referenced in: https://developers.google.com/web/tools/chrome-devtools/evaluate-performance/ -->

<!doctype html>
<html>
  <head>
    <!-- <link rel="shortcut icon" href="/devtools-samples/favicon-96x96.png"/>  -->
    <link rel="shortcut icon" href="faviconV2.png"/>
    <title>Janky Animation</title>
    <meta name="viewport" content="width=device-width, initial-scale=1">
    <link rel="stylesheet" type="text/css" href="styles.css"/>
    <script src="index.js" async></script>
  </head>
  <body>
    <img class="proto mover" src="faviconV2.png"/>
    <div class="controls">
      <button class="add"></button>
      <button class="subtract" disabled></button>
      <button class="stop">Stop</button>
      <button class="optimize">Optimize</button>
      <a href="https://www.educative.io/collection/page/10370001/6006542571667456/4821072372301824"
         target="_blank">
        <button class="optimize">Help</button>
      </a>
    </div>
  </body>
</html>
```
- index.js
```javascript
/* Copyright 2016 Google Inc.
 *
 * Licensed under the Apache License, Version 2.0 (the "License");
 * you may not use this file except in compliance with the License.
 * You may obtain a copy of the License at
 *
 * http://www.apache.org/licenses/LICENSE-2.0
 *
 * Unless required by applicable law or agreed to in writing, software
 * distributed under the License is distributed on an "AS IS" BASIS,
 * WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND, either express or implied.
 * See the License for the specific language governing permissions and
 * limitations under the License. */

(function(window) {
  'use strict';

  var app = {},
      proto = document.querySelector('.proto'),
      movers,
      bodySize = document.body.getBoundingClientRect(),
      ballSize = proto.getBoundingClientRect(),
      maxHeight = Math.floor(bodySize.height - ballSize.height),
      maxWidth = 97, // 100vw - width of square (3vw)
      incrementor = 10,
      distance = 3,
      frame,
      minimum = 10,
      subtract = document.querySelector('.subtract'),
      add = document.querySelector('.add');

  app.optimize = false;
  app.count = minimum;
  app.enableApp = true;

  app.init = function () {
    if (movers) {
      bodySize = document.body.getBoundingClientRect();
      for (var i = 0; i < movers.length; i++) {
        document.body.removeChild(movers[i]);
      }
      document.body.appendChild(proto);
      ballSize = proto.getBoundingClientRect();
      document.body.removeChild(proto);
      maxHeight = Math.floor(bodySize.height - ballSize.height);
    }
    for (var i = 0; i < app.count; i++) {
      var m = proto.cloneNode();
      var top = Math.floor(Math.random() * (maxHeight));
      if (top === maxHeight) {
        m.classList.add('up');
      } else {
        m.classList.add('down');
      }
      m.style.left = (i / (app.count / maxWidth)) + 'vw';
      m.style.top = top + 'px';
      document.body.appendChild(m);
    }
    movers = document.querySelectorAll('.mover');
  };

  app.update = function (timestamp) {
    for (var i = 0; i < app.count; i++) {
      var m = movers[i];
      if (!app.optimize) {
        var pos = m.classList.contains('down') ?
            m.offsetTop + distance : m.offsetTop - distance;
        if (pos < 0) pos = 0;
        if (pos > maxHeight) pos = maxHeight;
        m.style.top = pos + 'px';
        if (m.offsetTop === 0) {
          m.classList.remove('up');
          m.classList.add('down');
        }
        if (m.offsetTop === maxHeight) {
          m.classList.remove('down');
          m.classList.add('up');
        }
      } else {
        var pos = parseInt(m.style.top.slice(0, m.style.top.indexOf('px')));
        m.classList.contains('down') ? pos += distance : pos -= distance;
        if (pos < 0) pos = 0;
        if (pos > maxHeight) pos = maxHeight;
        m.style.top = pos + 'px';
        if (pos === 0) {
          m.classList.remove('up');
          m.classList.add('down');
        }
        if (pos === maxHeight) {
          m.classList.remove('down');
          m.classList.add('up');
        }
      }
    }
    frame = window.requestAnimationFrame(app.update);
  }

  document.querySelector('.stop').addEventListener('click', function (e) {
    if (app.enableApp) {
      cancelAnimationFrame(frame);
      e.target.textContent = 'Start';
      app.enableApp = false;
    } else {
      frame = window.requestAnimationFrame(app.update);
      e.target.textContent = 'Stop';
      app.enableApp = true;
    }
  });

  document.querySelector('.optimize').addEventListener('click', function (e) {
    if (e.target.textContent === 'Optimize') {
      app.optimize = true;
      e.target.textContent = 'Un-Optimize';
    } else {
      app.optimize = false;
      e.target.textContent = 'Optimize';
    }
  });

  add.addEventListener('click', function (e) {
    cancelAnimationFrame(frame);
    app.count += incrementor;
    subtract.disabled = false;
    app.init();
    frame = requestAnimationFrame(app.update);
  });

  subtract.addEventListener('click', function () {
    cancelAnimationFrame(frame);
    app.count -= incrementor;
    app.init();
    frame = requestAnimationFrame(app.update);
    if (app.count === minimum) {
      subtract.disabled = true;
    }
  });

  function debounce(func, wait, immediate) {
    var timeout;
    return function() {
      var context = this, args = arguments;
      var later = function() {
        timeout = null;
        if (!immediate) func.apply(context, args);
      };
      var callNow = immediate && !timeout;
      clearTimeout(timeout);
      timeout = setTimeout(later, wait);
      if (callNow) func.apply(context, args);
    };
  };

  var onResize = debounce(function () {
    if (app.enableApp) {
        cancelAnimationFrame(frame);
        app.init();
        frame = requestAnimationFrame(app.update);
    }
  }, 500);

  window.addEventListener('resize', onResize);

  add.textContent = 'Add ' + incrementor;
  subtract.textContent = 'Subtract ' + incrementor;
  document.body.removeChild(proto);
  proto.classList.remove('.proto');
  app.init();
  window.app = app;
  frame = window.requestAnimationFrame(app.update);

})(window);
```
- style.css
```css
/* Copyright 2016 Google Inc.
 *
 * Licensed under the Apache License, Version 2.0 (the "License");
 * you may not use this file except in compliance with the License.
 * You may obtain a copy of the License at
 *
 * http://www.apache.org/licenses/LICENSE-2.0
 *
 * Unless required by applicable law or agreed to in writing, software
 * distributed under the License is distributed on an "AS IS" BASIS,
 * WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND, either express or
 * implied. See the License for the specific language governing permissions
 * and limitations under the License. */

* {
  margin: 0;
  padding: 0;
}

body {
  height: 100vh;
  width: 100vw;
}

.controls {
  position: fixed;
  top: 2vw;
  left: 2vw;
  z-index: 1;
}

.controls button {
  display: block;
  font-size: 1em;
  padding: 1em;
  margin: 1em;
  background-color: beige;
  color: black;
}

.subtract:disabled {
  opacity: 0.2;
}

.mover {
  height: 3vw;
  position: absolute;
  z-index: 0;
}

.border {
  border: 1px solid black;
}

@media (max-width: 600px) {
  .controls button {
    min-width: 20vw;
  }
}
```

You’ll see a bunch of Educative icons moving around the screen. You have the options to:

- Add 10: This will add 10 more Educative icons. Keep adding 10 icons until you see a noticeable degradation in the performance. The icons will start to move slowly and there will be a noticeable jank.

- Optimize: When you hit the optimize button, after adding a few icons, there should be a clear increase in the speed of the squares and an improvement in the jank. Sometimes, it can take adding 250 more icons to see a difference, so don’t worry if you have to add a lot.

- Subtract 10: This will remove 10 icons.

- Stop / Start: This will stop or start the demo.

- Help: This will take you to this lesson. Note that this won’t work in incognito mode! Open the app on a regular tab to see this option.

## Open DevTools
- Press Command+Option+I (Mac) or Control+Shift+I (Windows, Linux) to open DevTools, OR right-click anywhere on the page and choose ‘inspect element.’
- You’ll see a bunch of tabs on the top. Click on ‘performance’.
The tab will currently be empty. You’ll need to record performance in order to see any data.

[Open DevTools](./devtool.jpg)

Note that you can click on the three dots on the top right and choose ‘dock side > undock into separate window.’ That’ll put the developer tools in a separate window, and that’s how we’ll show the rest of the screenshots in this lesson.

[Recording](./recording.jpg)

Next, add 10 a few times and click on optimize. Do this until you notice a difference between the optimized and unoptimized versions.

## Record
Next, hit ‘record,’ and wait for a few seconds. 5-10 seconds is good enough for our purposes. Stop recording and let it load up the data.

[Recording](./rec.jpg)

[Loading](./loading.jpg)

## Data
What you’ll end up seeing is something like this. Now, don’t freak out. We know it’s a lot to take in, but we’ll break these results down into digestible chunks and explain everything step-by-step in the next lesson.

[Data](./data.jpg)
