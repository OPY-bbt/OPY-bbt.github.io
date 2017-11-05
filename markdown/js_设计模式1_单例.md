# Single mode


`定义： 保证一个类仅有一个实例，并提供一个访问它的全局访问点`

### 一个简单实现
```javascript
const Singleton = function(name) {
    this.name = name;
    this.instance = null;
};

Singleton.getInstance = function(name) {
    if (!this.instance) {
        this.instance = new Singleton(name);
    }
    return this.instance;
}

const a = Singleton.getInstance('z');
const b = Singleton.getInstance('y');

console.log(a === b);
```

### 透明的单例
***像一个普通对象一样使用的单例, 创建一个全局唯一的div***
```javascript
var CreateDiv = (function() {
    var instance = null;

    var CreateDiv = function(html) {
        if (instance) {
            return instance;
        }

        this.html = html;
        this.init();
        return instance = this;
    };

    CreateDiv.prototype.init = function() {
        var div = document.createElement('div');
        div.innerHTML = this.html;
        document.body.appendChild(div);
    }

    return CreateDiv;
})();

var a = new CreateDiv('z');
var b = new CreateDiv('y');

console.log(a === b);
```

### 用代理实现单例，模块化 
```javascript
var CreateDiv = function(html) {
    this.html = html;
    this.init();
}

CreateDiv.prototype.init = function() {
    var div = document.createElement('div');
    div.innerHTML = this.html;
    document.body.appendChild(div);
}

var ProxySingletonCreateDiv = (function(){
    var instance = null;

    return function(html) {
        if (!instance) {
            instance = new CreateDiv(html);
        }

        return instance;
    }
})()
```

### 惰性单例 
```javascript
var getSingle = function(fn) {
    var result = null;

    return result || (result = fn.apply(this, arguments));
};

var createDiv = function(html) {
    var div = document.createElement('div');
    div.innerHTML = html;
    document.body.appendChild(div);
}

var crateSingleDiv = getSingle(createDiv);

createSingleDiv();
```





