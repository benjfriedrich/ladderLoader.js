# Ladder.loader()

Ladder.loader() is an image loading function for static-structured sites. It runs on the client's web-browser and loads images one at time, but gives you the power to prioritize the download queue by assigning images to tiers. 

That means the images in the first tier will load one-by-one, top-to-bottom, and then the second tier will start to load, and so forth. This approach provides a *faster feeling* loading experience than default batch loading and lets you set priorities, while keeping it simple. 

**That's why it's called Ladder.loader(). It's like a ladder, and it loads.**

**Crucially, Ladder.loader() is also not a lazy-loader.** Lazy-loaders load images based on your scroll position, so it only loads images when you are about to see to them. Lazy-loading is totally cool and useful, but it's not always snappy feeling in practice. **Lazy-loading can be a good technique, but Ladder.loader() is a heavy-lifting, blue-collar kind of loader.**

## How?

Using Ladder.loader() has 2 requirements: **1)** you'll need to properly format the relevant \<img\> tags in the site's raw HTML, and **2)** you'll need to call Ladder.loader() using JavaScript from the browser.

### In HTML:

**1)** Assign your \<img\> tags to tiers by class,
**2)** assign the actual link to a data slot,
**3)** & fill the src with a temporary image. 
	
```HTML
<img class="first-tier" data-llsrc="/images/fullImage.jpg" src="/images/tempLoading.svg">
```

**Also, you'll need to import the ladderLoader.js in your header or footer.** You can host the file yourself, or pull from a CDN.

```HTML
<footer>
<script type="text/javascript" src="https://cdn.jsdelivr.net/gh/benjfriedrich/ladderLoader.js@1.0/ladderLoader.min.js"></script>
</footer>
```

### In JavaScript:

**1)** Define a tier using a 2-element array of stings containing **a)** the tier's class, and **b)** the relevant data-attribute, **or** pass in the tier's class as a string and 'llsrc' will be used as the default data-attribute.

```javascript 
['first-tier','llsrc'] 
```

**2)** pass as many of these arguments into Ladder.loader() as you want, in the order that you want them loaded,

```javascript 
Ladder.loader(['first-tier','llsrc'],['second-tier','llsrc'],['third-tier','llsrc'])
```

**3)** & then call Ladder.loader() at the appropriate time!

```javascript 
window.onload = Ladder.loader(['first-tier','llsrc'])
```

## Pro-tips:

Remember you can pass functions that return strings as arguments. A common use for this would be to provide smaller versions of files to mobile devices.

**In HTML:**

```HTML 
<img class="first-tier" data-llsrcFull="/fullImage.jpg" data-llsrcMobile="/smallImage.jpg" src="/images/tempLoading.svg"> 
```

**In JavaScript:**

```javascript 
const isMobile = () => window.innerWidth <= 800 ? "llsrcMobile" : "llsrcFull";

window.onload = Ladder.loader(['first-tier',isMobile()])
```

Also, window.onload is an obvious moment to run a Ladder.loader(), but you can trigger it any way you want, and you could also trigger multiple instances of the function based on different conditions. **It's a really simple function, so you can be creative with it.**
