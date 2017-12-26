# 栈 LIFO


## 实现
```js
let Stack = (function() {
    // 私有变量
    const items = new WeakMap();

    class Stack {
        constructor() {
            items.set(this, []);
        }
        push(ele) {
            const _item = items.get(this);
            _item.push(ele);
        }
        pop() {
            return items.get(this).pop();
        }
        peek() {
            const _item = items.get(this);
            return _items[_item.length - 1];
        }
        isEmpty() {
            return items.get(this).length === 0;
        }
        clear() {
            items.set(this, []);
        }
        print() {
            console.log(items.get(this));
        }
        size() {
        	return items.get(this).length;
        }
    }

    return Stack;
})()
```

## 应用

`进制转换`
```js
const divideBy2 = (decNumber) => {
	let remStack = new Stack();
  	let rem;
    let binaryString = '';
  
  	while (decNumber > 0){
          rem = decNumber % 2;
          remStack.push(rem);
          decNumber = Math.floor(decNumber / 2)
    }

    while (!remStack.isEmpty()) {
        binaryString += remStack.pop().toString();
    }

    return binaryString;
}
```

`括号有效性`
```js
const parenthesesChecker = (symbols) => {
    let stack = new Stack(),
    balance = true,
    index=0,
    symbol,
    top,
    opens = "([{",
    closers = ")]}";

    while(index < symbols.length && balance) {
        symbol = symbols.charAt(index);
        if (opens.indexOf(symbol)>-1) {
            stack.push(symbol);
            index++;
        } else {
            if (stack.isEmpty()) {
                balance = false;
            } else {
                top = stack.pop();
                if(opens.indexOf(top) === closers.indexOf(symbol)) {
                    index++;
                } else {
                    balance = false;
                }
            }
        }
    }

    if (balance && stack.isEmpty()) {
        return true;
    }
    return false;
}
```

`汉诺塔 Hanoi`
```js 
const towerOfHanoi = (n, from, dest, helper) => {
  if (n > 0) {
    towerOfHanoi(n - 1, from, helper, dest);
    dest.push(from.pop());
    towerOfHanoi(n - 1, helper, dest, from);
  }
    
}

var source = new Stack();
source.push(3);
source.push(2);
source.push(1);

var dest = new Stack();
var helper = new Stack();

towerOfHanoi(source.size(), source, dest, helper);

source.print();
dest.print();
helper.print();
```


