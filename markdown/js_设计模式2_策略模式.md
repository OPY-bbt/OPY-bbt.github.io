# strategy mode


`定义： 定义一系列算法，把它们一个个封装起来，并且使它们可以相互替换`
`主要使用在动画和表单验证`

### 动画
```javascript
// 定义一些算法 
var tween = {
    //...
}
var Animate = function(dom) {
    // 记录一些值，起点，终点，持续时间。。
}
Animate.prototype.start = function(prototype, endPos, duration, easing) {
    // 记录值 ....
    var self = this;
    var timeId = setInterval(function() {
        if (self.step() === false) {
            clearInverval(timeId);
        }
    }, 19);
}
Animate.prototype.step = function(){
    // 记录时间 ...
    // 判断时间是否有剩余，若没有，返回false；

    var pos = this.easing();
    this.update(pos);
}
Animate.prototype.update = function() {
    this.dom.style[prototype] = pos;
}
```

### 表单验证
```javascript
var strategies = {
    isNonEmpty: function(value, errorMsg) {
        if (value === '') {
            return errorMsg;
        }
    }
    // ......
};
function Validator() {
    this.cache = [];
}
Validator.prototype.add = function(dom, rule, errorMsg) {
    var ary = rule.split(':');
    this.cache.push(function() {
        var strategy = ary.shift();
        ary.unshift(dom.value);
        ary.push(errorMsg);
        strategies[rule].apply(dom, ary);
    });
}
Validator.prototype.start = function() {
    for( var i = 0, validatorFunc; validatorFunc = this.cache[i++];) {
        var msg = validatorFunc();
        if (msg) {
            return msg;
        }
    }
}

var ValidataFunc = function() {
    var validator = new Validator();
    validator.add(dom, 'isNonEmpty', 'username is can\'t empty');
    // ....

    var errormsg = validator.start();
    return errormsg;
}
```