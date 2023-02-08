# Content Optimizations
## Minifying
One way to load pages faster is to reduce the number of requests.

JavaScript, CSS, and HTML files can all be compressed and turned into one file. All unnecessary spaces, line indents, and code are removed as part of the process of minifying. This significantly reduces the size of the files to be transferred, and hence saves bandwidth and speeds up your site. This is a common pre-deployment practice.

Check out this example.

### Before minifying
```css
    var order = {
      "date": "11/20/2013",
      "customerId": 116,
      "items": [
        {
          "product": "Surface 4 Pro",
          "unitprice": "799",
          "amount": 1
        },
        {
          "product": "Type Cover 4",
          "unitprice": "129",
          "amount": 1
        },
        {
          "product": "Docking station",
          "unitprice": "199",
          "amount": 1
        }
      ]
    }
    console.log("Date: " + order.date);
    for (var i = 0; i < order.items.length; i++) {
      console.log("Product: "
        + order.items[i].product);
      console.log("Unit price: "
        + order.items[i].unitprice);
      console.log("Amount: "
        + order.items[i].amount);
    }
```
### After minifying
```css
var order={"date":"11/20/2013","customerId":116,"items":[{"product":"Surface 4 Pro","unitprice":"799","amount":1},{"product":"Type Cover 4","unitprice":"129","amount":1},{"product":"Docking station","unitprice":"199","amount":1}]}
console.log("Date: "+order.date);for(var i=0;i<order.items.length;i++){console.log("Product: "+order.items[i].product);console.log("Unit price: "+order.items[i].unitprice);console.log("Amount: "+order.items[i].amount)}
```
## Run a compression audit
The smaller the size of your files, the fewer HTTP requests made, the quicker they’ll get delivered to the client, and the faster your site will load.

So, it is a natural goal to reduce your file sizes as much as possible without compromising their quality.

Your HTML/JS/CSS files can be compressed in addition to being minified. Gzip is a tool that helps with this. It reduces all repeated code and whitespaces and temporarily replaces them to make the file smaller. Note that Gzip does not work with images. We will discuss image compression in the next section. The server compresses the files via Gzip before sending them to the browser, and the browser unzips the files and presents the contents.

There are a bunch of tools available online that can be used to check or ‘audit’ the state of compression on your website. Some even report the percentage of files compressed out of a total that could be. giftofspeed is one such tool. We tried educative.io on it, here is the result we got:

[Result of auditing educative.io for compression]



## Reduce image sizes
High-quality, eye-catching images are incredibly important to engage a user. However, images are large files, and they take up a significant percentage of the size of the files transferred for an average website. Hence, it makes sense to reduce their size without compromising on quality. The best way to do this is to compress images using tools such as ImageOptim, JPEGmini, TinyPNG, or PhotoShop.

Also, aim for vector formats, because vector images are resolution and scale independent. This can really simplify things, because your website can then be rendered on any device on any resolution.

Always minify and compress your SVG images. These text-based ‘images’ contain a lot of metadata that can become a bottleneck to performance. The same goes for JPEG images. They usually contain metadata like geo information, camera information, when the photo was taken, etc. that may not be critical to your website.

## Reduce the number of plugins
Plugins allow additional functionality to be added to a website with relative ease. However, the more plugins that are installed, the more resources they will consume. Hence, it’s best to keep them to a minimum.

Try auditing your website and listing all the plugins used. Eradicate the unnecessary ones and keep only the most important ones.
