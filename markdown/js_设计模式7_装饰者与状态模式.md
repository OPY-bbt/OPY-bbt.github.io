# Decorator mode

`给对象动态的增加职责`

### 用AOP实现装饰函数
```javascript
Function.prototype.before = function(beforefn) {
    var _self = this;
    return function() {
        beforefn.apply(this, arguments);
        return _self.apply(this, arguments);
    }
}
Function.prototype.after = function(afterfn) {
    var _self = this;
    return function() {
        var ret = _self.apply(this, arguments);
        return ret;
    }
}
```

### 状态模式

`某些内部状态的改变会导致对象行为的改变`

### 文件上传。类似云盘上传文件，在文件扫描过程中，是不能操作的，上传过程中可以点击暂停按钮来暂停上传，再次点击可以继续上传，扫描和上传过程中，点击删除按钮无效。这就是一个典型的根据内部状态改变行为的场景。
```javascript
var Upload = function(filename) {
    this.plugin = plugin;
    this.filename = filename;
    this.btn1 = null;
    this.btn2 = null;

    // 每一种状态都对应于一个类的实例
    this.scanState = new ScanState(this);
    //...

    // 初始状态
    this.curState = this.scanState;
}

Upload.prototype.init = function() {
    var that = this;

    //初始化html, 绑定事件。
}
Upload.prototype.bindEvent = function() {
    var self = this;
    this.btn1.onclick = function() {
        self.currState.clickHandler1();
    }
    // btn2
}
// 状态对应的逻辑行为
Upload.prototype.scan = function() {}
// ...

// 各个状态类
var StateFactory = (function(){
    var State = function() {};

    State.prototype.clickHandler1 = function() {
        throw new Error('子类需重写父类1');
    }
    State.prototype.clickHandler2 = function() {
        throw new Error('子类需重写父类2');
    }

    return function(param) {
        var F = function(uploadObj) {
            this.uploadObj = uploadObj;
        };
        F.prototype = new State();
        
        for (var i in param) {
            F.prototype[i] = param[i];
        }
        return F;
    }
})();
var SignState = StateFactory({
    clickHandler1: function(){},
    clickHandler2: function(){}
});
// 其余状态类 ...
```

### 上面的状态机是模拟传统面向对象语言状态模式实现。下面一个简单开灯关灯状态机的js实现方法
```javascript
    var Light = function() {
        this.curState = FSM.off;
        this.button = null;
    }

    Light.prototype.init = function() {
        // dom, html;
        var self = this;
        this.button.onclick = function() {
            self.currState.buttonWasPressed.call(self);
        }
    }

    var FSM = {
        off: {
            buttonWasPressed: function() {
                console.log('close');
                this.curState = FSM.on;
            }
        },
        on: {
            buttonWasPressed: function() {
                console.log('on');
                this.curState = FSM.off;
            }
        }
    }
```

### 另外一个🌰, 游戏中人物在走动时可以攻击，攻击中不能走动。（好像怪怪的）
```javascript
var FSM = {
    walk: {
        attack: function() {
            log('attack');
        }
    },
    attack: {
        walk: function() {
            log('can\'t walk');
        }
    }
}
```