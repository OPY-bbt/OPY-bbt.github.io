# iterator mode


`内部迭代器, 外部迭代器`

```javascript
    function each(ary, callback) {
        for (var i=0, l=ary.length;i<l;i++) {
            callback.call(ary[i], i, ary[i])
        }
    }
```

### 外部迭代器
```javascript
    var Iterator = function(obj) {
        var current = 0;

        var next = function() {
            current += 1;
        };

        var isDone = function() {
            return current >= obj.length;
        };

        var getCurrentItem = function() {
            return obj[current];
        }

        return {
            next,
            isDone,
            getCurrentItem,
            length: obj.length,
        }
    }

    function compare (iterator1, iterator2) {
        if (iterator1.length !== iterator2.length) {
            alert('not equal');
        }

        while( !iterator1.isDone() && !iterator2.isDone()) {
            if (iterator1.getCurrentItem() !== iterator2.getCurrentItem()) {
                alert('not equal');
            }
            iterator1.next();
            iterator2.next();
        }
        alert('equal');
    }
```

### 迭代器模式实践
```javascript
    function getUploadObj() {
        try {
            return new ActiveXObject('xxxx');
        } catch(e) {
            if (supportFlash()) {
                var str = '<object type="application"/x-shockwave-flash"></object>';
                return $('str').appendTo($('body'));
            } else {
                var str = '<input name="file" type="file" />';
                return $(str).appendTo($('body'));
            }
        }
    }

    // 使用迭代器更好的处理方式
    function getActiveXObj() {
        try {
            return new ActiveXObject('xxxx');
        } catch(e) {
            return false;
        }
    }

    function getFalshuploadObj () {
        // ...
    }
    
    function getFormuploadObj() {
        // ...
    }

    var iteratorUploadObj = function() {
        for ( var i = 0, fn; fn = arguments[i++]; ) {
            var uploadobj = fn();
            if (uploadobj !== false) {
                return uploadobj;
            }
        }
    }
    iteratorUploadObj(
        getActiveXObj,
        getFalshuploadObj,
        getFormuploadObj)
```
