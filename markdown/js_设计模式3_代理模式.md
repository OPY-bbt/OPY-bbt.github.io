# proxy mode


`定义: 为对象提供一个代用品或占位符，以便控制对它的访问。`

### 虚拟代理实现图片预加载
```javascript
    var myImg = (function(){
        var imgNode = document.createElement('img');
        document.body.appendChild(imgNode);

        return {
            setSrc: function(src) {
                imgNode.src = src;
            }
        };
    })();

    var proxyImg = (function() {
        var img = new Img();
        img.onload = function() {
            myImg.setSrc(this.src);
        }
        return {
            setSrc: function(src) {
                img.src = src;
                myImg.setSrc('loading.png');
            }
        }
    })();

    proxyImg.setSrc('xxxx.png');
```

### 虚拟代理合并请求(这个应该会有问题吧。。。)
```javascript
    var synchronousFile = function(id) {
        console.log('sync file');
    }

    var proxySynchronousFile = (function(){
        var cache = [],
            timer;

        return function(id) {
            cache.push(id);
            if (timer) {
                return;
            }

            timer = setTimeout(function(){
                synchronousFile(cache.join(','));
                clearTimeout(timer);
                cache.length = 0;
                timer = null;
            }, 2000);
        }

    })()
```

### 虚拟代理在惰性加载中的应用
***一个懒加载的log窗口***
***在加载js之前先把执行代码，存起来，待加载完执行***
```javascript
    // 加载之前
    var cache = [];
    var miniConsole = {
        log: function() {
            var args = arguments;
            cache.push( function() {
                return miniConsole.log.apply(miniConsole, args);
            });
        }
    }

    var handler = function(ev) {
        if (ev.keyCode === 113) {
            var script = document.createElement('script');
            script.onload = function() {
                for( var i = 0, fn; fn = cache[i++]; ) {
                    fn();
                }
            }
            script.src = 'xx.js';
            document.getElementsByTagName('head')[0].appendChild(script);
        }
    };

    document.body.addEventListener('keydown', handler, false);

    // real js
    miniConsole = {
        log: function() {
            console.log(Array.prototype.join.call(arguments));
        }
    }
```

### 缓存代理
***对于计算量大的计算， 可以将计算结果缓存起来***
```javascript
    var mult = function() {
        var a = 1;
        for( var i = 0; i<arguments.length; i++) {
            a = a * arguments[i];
        }
        return a;
    };

    var proxyMult = (function() {
        var cache = {};

        return function() {
            var args = Array.prototype.join.call(arguments, ',');
            if (cache[args]) {
                return cache[args];
            }
            var res = mult.apply(this, arguments);
        }
    })();
```
