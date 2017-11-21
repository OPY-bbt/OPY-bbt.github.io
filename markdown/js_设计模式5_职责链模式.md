# Responsibility chain mode


`定义： 使多个对象都有机会处理请求，从而避免请求的发送者和接受者之间的耦合。`

### 实际开发中的例子
```javascript
// 手机商场售卖手机，对于付过500元定金的可以收到100元抵用卷，200元定金可以收到50元定金，没有定金的只能通过普通售卖，且库存不足还无法购买到

function order(type, pay, stock) {
    if (type === 0) {   // 500
        if (pay === true) { // paid
            log('100 coupon')；
        } else {
            if (stock > 0) {
                // have many
            } else {
                // done
            }
        }
    } else if (type === 1) { // 200
        // ...
    } else if (type === 2) { // no earnest
        // ...
    }
}
// 很可能写出这样的代码
```

### 稍微优化一下可能就是这个样子
```javascript
function order500(type, pay, stock) {
    if (type === 0 && pay === true) {
        log('100 coupon')；
    } else {
        order200(type, pay, stock);
    }
}

function order200(type, pay, stock) {
    // ...
}

function orderNormal(type, pay, stock) {
    // ...
}

// 这样逻辑上清晰了很多，但是职责链很僵硬，传递请求耦合爱一起。下面用职责链重构代码。
```

### 首先保证几个处理函数要相互独立，所以我们约定如果函数不满足条件久就返回‘next’，交给下一个函数处理。这样我们只要定义个函数链就可以解决问题。

```javascript
function order500(type, pay, stock) {
    if (type === 0 && pay === true) {
        log('100 coupon')；
    } else {
        return 'next';
    }
}

function order200() {}

function orderNormal() {}

// 建立职责链
var Chain = function(fn) {
    this.fn = fn;
    this.next = null;
}
Chain.prototype.setNext = function(fn) {
    this.next = fn;
}
Chain.prototype.passRequest = function() {
    var ret = this.fn.apply(this, arguments);
    if (res === 'next') {
        return this.next && this.next.passRequest.apply(this, argument);
    }
    return ret;
}

//使用
var chainOrder500 = new Chain(order500);
var chainOrder200 = new Chain(order200);
var chainOrderNormal = new Chain(orderNormal);

chainOrder500.setNext(chainOrder200);
chainOrder200.setNext(chainOrderNormal);

chainOrder500.passRequset([type], [pay], [stock]);
// 这样就可以实现灵活增加节点顺序。
```

### 用AOP实现职责链
```javascript
Function.prototype.after = function(fn) {
    var self = this;
    return function() {
        var ret = self.apply(this, arguments);
        if ( ret === 'next') {
            return fn.apply(this, arguments)
        }
        return ret;
    }
}

var order = chainOrder500.after(chainOrder200).after(chainOrderNormal);
// 这样的实现会更加简练
```