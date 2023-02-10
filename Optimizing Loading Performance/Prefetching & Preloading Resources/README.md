# Prefetching & Preloading Resources
Prefetching something is requesting it ahead of time. A good analogy would be ordering extra pizza for a birthday party in case some extra guests choose to show up. The pizza may eventually not be needed, but you still have it just in case. That’s exactly how prefetching works on websites. There are a few kinds of prefetching. Let’s discuss each.

## Asset / link prefetching
Assets that may eventually be needed can be preloaded using the HTML <link> tag and the prefetch directive as follows. These are assets that may be required by the next user navigation. So, for example, they may belong to a page that is linked in the current page.

Consider the following example. The blue Educative icon is already preloaded on line 5 of the HTML. So, when you click on the actual link, it is served very quickly because it was already loaded.

Open up your network tab before hitting ‘run’. You’ll notice that a request for the icon is fired off even though it’s not actually used on this page.

[Loading the HTML fires off a request for the asset. Note that this is from Firefox's inspector tool.](./p.jpg)

```html
<html>
 <head>
 </head>
 <body>
   <link rel="prefetch" href="/udata/1kMwYObLWGB/blue_logo.png"/>
    <a href="/udata/1kMwYObLWGB/blue_logo.png">Educative's blue logo</a> </body>
</html>
```

## DNS prefetching
DNS resolution, or figuring out what IP address belongs to what domain name, can have a serious network overhead. In other words, it can take a lot of time and requests to find the IP address for a domain name, and during this time, the browser is blocked. So, if a user visits your site and clicks on a link, the domain name will have to be resolved before they can actually visit the website that is linked.

This time can potentially be saved with DNS prefetching. Have a look at the code example below that does this. The <link> tag with the dns-prefetch attribute is used to achieve this.

```html
<html>
 <head>
 </head>
 <body>
   <link rel="dns-prefetch" href="https://www.educative.io/blog">
   <a href="https://www.educative.io/blog">Educative Blogs</a>
 </body>
</html>
```

## Prerendering
Prerendering is completely loading and rendering a given link on a page in the background before the user actually clicks it. When the user actually clicks on the link, the window shown is simply swapped for the pre-rendered one. Have a look at the following example of this:

```html
<html>
 <head>
 </head>
 <body>
   <link rel="prerender" href="https://www.educative.io/blog">
   <a href="https://www.educative.io/blog">Educative Blogs</a>
 </body>
</html>
```
