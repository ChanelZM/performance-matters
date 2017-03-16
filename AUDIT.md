# Audit
## Images
![before using picture and webp](https://github.com/ChanelZM/performance-matters/blob/feature/images/auditimg/beforepicture.png)
![before using picture and webp](https://github.com/ChanelZM/performance-matters/blob/feature/images/auditimg/afterpicture.png)

### What I did
First I compressed the images. Then I checked my website with [GT Metrix](https://gtmetrix.com/) and it told me that I could resize my images to 234x176. Then I changed the HTML from:
```html
<img alt="Lyft" src="/assets/img/expo-lyft-big.jpg" class="img-responsive">
```
To:
```html
<picture>
<source type="image/webp" srcset="/assets/img/expo-lyft-big.webp" media="(min-width: 1200px)">
<source type="image/webp" srcset="/assets/img/expo-lyft.webp">
<source srcset="/assets/img/expo-lyft-big.jpg" media="(min-width: 1200px)">
<source srcset="/assets/img/expo-lyft.jpg">
<img alt="Lyft" src="/assets/img/expo-lyft-big.jpg" class="img-responsive">
</picture>
```