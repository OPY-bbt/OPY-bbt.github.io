# self.createImageBitmap()

## work in global and workers

## Syntax
- createImageBitmap(image[, options]).then(function(response) { ... })
- createImageBitmap(image, sx, sy, sw, sh[, options]).then(function(response) { ... })

## params
- image: An image source. `<img>, SVG<image>, <video> ???, <canvas>, Blob`
- sx
- sy
- sw
- sh
- option
- - imageOrientation [none, flipY]
- - premultiplyAlpha (https://segmentfault.com/a/1190000002990030) [none, premultiply, default]
- - colorSpaceConversion [none, default]
- - resizeWidth (chrome 68 对resize支持还是有问题)
- - resizeHeight
- - resizeQuality [pixelated, low (default), medium, or high]

## example
```html
<body>
  <img src="./me.jpg" />
  <canvas width="400" height="400"></canvas>
  <script>
    window.onload = function() {
      const img = document.querySelector('img');
      const canvas = document.querySelector('canvas');
      const ctx = canvas.getContext('2d');

      createImageBitmap(img, {
        imageOrientation: 'flipY',
        premultiplyAlpha: 'none',
        resizeWidth: 200,
        resizeHeight: 200,
        resizeQuality: 'high',
      }).then((d) => {
        ctx.drawImage(d, 200, 200);
      });
    }
  </script>
</body>
```

