# Audit
## Images
### How it was
Bootstrap uses one image element which is pretty big, both size and weight. This of course results in long loads when your internet connection isn't the best. Plus, big images are not needed on smaller screens so resizing them for smaller screens is better practice.

###### The code
```html
<img alt="Lyft" src="/assets/img/expo-lyft-big.jpg" class="img-responsive">
```

###### Loading time
![before using picture and webp](https://github.com/ChanelZM/performance-matters/blob/feature/js/auditimg/beforepicture.png)

### What I did
To tackle this problem, I first compressed the images using [Tiny JPG](https://tinyjpg.com/). The pictures became significantly smaller. The second step I took was analyzing my website with [GT Metrix](https://gtmetrix.com/) (Credits to [Elton Goncalves Gomes](https://github.com/eltongonc)). This website told me that resizing the images would make the website faster. I copied and changed the images from 800px wide to 234px wide. So then I had two sizes which I placed in a `<picture>` element and added a `<source srcet="">`. I also converted the images to WEBP and placed them inside the `<picture>` element.

###### The code
```html
<picture>
<source type="image/webp" srcset="/assets/img/expo-lyft-big.webp" media="(min-width: 1200px)">
<source type="image/webp" srcset="/assets/img/expo-lyft.webp">
<source srcset="/assets/img/expo-lyft-big.jpg" media="(min-width: 1200px)">
<source srcset="/assets/img/expo-lyft.jpg">
<img alt="Lyft" src="/assets/img/expo-lyft-big.jpg" class="img-responsive">
</picture>
```

###### Loading time
![before using picture and webp](https://github.com/ChanelZM/performance-matters/blob/feature/js/auditimg/afterpicture.png)

## CSS
### How it was
Bootstrap loads all the CSS from the folder but doesn't use a CDN for the job. The CSS also isn't minified so I takes quite a long time to load.

###### The code
```html
<link href="/dist/css/bootstrap.css" rel="stylesheet">
```

###### Loading time
![before changing the css](https://github.com/ChanelZM/performance-matters/blob/feature/js/auditimg/afterpicture.png)

### What I did
First I changed the link to bootstrap.css into an external bootstrap.css on a CDN (Source why I did this: [Elton Goncalves Gomes](https://github.com/eltongonc)). Then I merged fonts.css and docs.css so the website doesn't do a lot of HTTP requests. And lastly I minified the CSS with an online CSS minifier called [CSS Compressor](http://csscompressor.com/). I will not include the minified CSS in this audit, because it is waaaaay to long and you get the point.

###### The code
```html
<link rel="stylesheet" href="https://maxcdn.bootstrapcdn.com/bootstrap/3.3.7/css/bootstrap.min.css" integrity="sha384-BVYiiSIFeK1dGmJRAkycuHAHRg32OmUcww7on3RYdg4Va+PmSTsz/K68vbdEjh4u" crossorigin="anonymous">
<link href="/assets/css/src/docs.min.css" rel="stylesheet">
```

###### Loading time
![After modifying the CSS](https://github.com/ChanelZM/performance-matters/blob/feature/js/auditimg/aftercss.png)

## JavaScript
### How it was
Just like the CSS, the scripts aren't loaded with a CDN. There is also possibility that the scripts interrupt content loading although they are placed at the bottom of the HTML file.

###### The code
```html
<script src="/assets/js/vendor/jquery.min.js" defer></script>
<script src="/dist/js/bootstrap.js" defer></script>
<script src="/assets/js/docs.min.js" def></script>
<script src="/assets/js/ie10-viewport-bug-workaround.js"></script>
```

###### Loading time
![Before modifying the JS](https://github.com/ChanelZM/performance-matters/blob/feature/js/auditimg/aftercss.png)

### What I did
I first changed the local links to JS into external JS from a CDN. I also placed a defer on the scripts and placed them in the head, so that it doesn't interrupt content loading.

###### The code
```html
<script src="https://code.jquery.com/jquery-3.1.1.min.js"
integrity="sha256-hVVnYaiADRTO2PzUGmuLJr8BLUSjGIZsDYGmIJLv2b8="
crossorigin="anonymous" defer></script>
<script src="https://maxcdn.bootstrapcdn.com/bootstrap/3.3.7/js/bootstrap.min.js" integrity="sha384-Tc5IQib027qvyjSMfHjOMaLkfuWVxZxUPnCJA7l2mCWNIpG9mGCD8wGNIcPD7Txa" crossorigin="anonymous" defer></script>
<script src="/assets/js/docs.min.js" defer></script>
<script src="/assets/js/ie10-viewport-bug-workaround.js" defer></script>
```

###### Loading time
![Before modifying the JS](https://github.com/ChanelZM/performance-matters/blob/feature/js/auditimg/afterjs.png)

## Iteration 1
After adding the above features I added a few more things: with Node.js I added a compressor to make the website even smaller, but I had to use the local JS files and couldn't load them from a CDN. And I added critical CSS using a Node.js module named Critical. The module made the critical css file for me which I then used inline to minimize blokking the DOM.

###### The code
HTML:
```html
<script src="../assets/js/vendor/loadcss.min.js"></script>
<script defer>loadCSS('/assets/css/src/docs.min.css'); loadCSS('../dist/css/bootstrap.css');</script>
<noscript>
<link rel="stylesheet" href="/assets/css/src/docs.min.css">
<link rel="stylesheet" href="../dist/css/bootstrap.css">
</noscript>
<style><!--inline style here--></style>
```

Javascript:
```javscript
const critical = require('critical');
const compression = require('compression');

app.use(compression({threshold: 0, filter: function(){return true;}}));//Credits to Merlijn Vos

critical.generate({//Credits to both NPMJS and Elton Goncalves Gomes.
inline: false,
base: 'src/',
src: 'index.html',
css: ['src/dist/css/bootstrap.css', 'src/assets/css/src/docs.min.css'],
width: 1300,
height: 900,
dest: 'dist/css/critical.css',
minify: true,
extract: true
});
```

###### The final loading time
![Final loading time audit](https://github.com/ChanelZM/performance-matters/blob/feature/js/auditimg/final.png)

