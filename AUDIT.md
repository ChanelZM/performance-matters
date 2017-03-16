# Audit
## Images
### How it was
Bootstrap uses just one image element which is pretty big, both size and weight. This ofcourse results in long loads when your internet connection isn't the best. Plus, big images are not needed on smaller screens so resizing them for smaller screens is ideal.

###### The code
```html
<img alt="Lyft" src="/assets/img/expo-lyft-big.jpg" class="img-responsive">
```

###### Loading time
![before using picture and webp](https://github.com/ChanelZM/performance-matters/blob/feature/images/auditimg/beforepicture.png)

### What I did
First I compressed the images using [Tiny JPG](https://tinyjpg.com/). Then I checked my website with [GT Metrix](https://gtmetrix.com/) and it told me that I could resize my images to 234x176. I placed the image inside a `<picture>` element and added a `<source srcet="">`. I also converted the images to WEBP and placed them inside the `<picture>` element.

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
![before using picture and webp](https://github.com/ChanelZM/performance-matters/blob/feature/images/auditimg/afterpicture.png)

## CSS
### How it was
Bootstrap doesn't even use CDN but loads the bootstrap.css from their own folder. Plus their css isn't minified.

###### The code
```html
<link href="/dist/css/bootstrap.css" rel="stylesheet">
```

###### Loading time
![before changing the css](https://github.com/ChanelZM/performance-matters/blob/feature/criticalcss/auditimg/afterpicture.png)

### What I did
First I changed the link to bootstrap.css to an external bootstrap.css on CDN. Then I merged fonts.css and docs.css. The last stap was minifying the css. I did that with [CSS Compressor](http://csscompressor.com/).

###### The code
```html
<link rel="stylesheet" href="https://maxcdn.bootstrapcdn.com/bootstrap/3.3.7/css/bootstrap.min.css" integrity="sha384-BVYiiSIFeK1dGmJRAkycuHAHRg32OmUcww7on3RYdg4Va+PmSTsz/K68vbdEjh4u" crossorigin="anonymous">
<link href="/assets/css/src/docs.min.css" rel="stylesheet">
```

###### Loading time
![After modifying the CSS](https://github.com/ChanelZM/performance-matters/blob/feature/criticalcss/auditimg/aftercss.png)