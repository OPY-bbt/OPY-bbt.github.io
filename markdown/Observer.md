# Observer
`最近在读reudx源码，发现其中使用到了 symbol-observable(完全不知道是干啥的)。所以在此记录一下学习的过程，希望对同学们有所帮助`

## symbol-observable
在 readme.md 中可以看到此库是对observable的ponyfill，是不是打错了ponyfill？？？这里没错ponyfill是一种非侵入式的polyfill，并不会影响到全局。感觉兴趣的同学，可以自行查看 ponyfill[https://github.com/sindresorhus/ponyfill]。

## observable
处于stage-1阶段的提案
### 先上例子，感受一下
```js
function listen(element, eventName) {
  return new Observable(observer => {
      // Create an event handler which sends data to the sink
      let handler = event => {
        observer.next(event);

        if (event.key === "1") {
          observer.complete();
        }
      }

      // Attach the event handler
      element.addEventListener(eventName, handler, true);

      // Return a cleanup function which will cancel the event stream
      return () => {
          // Detach the event handler from the element
          element.removeEventListener(eventName, handler, true);
      };
  });
}

function commandKeys(element) {
  return listen(element, "keydown")
}

const inputElement = document.querySelector('#input');
commandKeys(inputElement).subscribe({
  next(e) { console.log("Received key command: " + e.key) },
  complete() { console.log("Stream complete") }
})

// 输入3213
// log：
//     Received key command: 3
//     Received key command: 2
//     Received key command: 1
//     Stream complete
```
可以看到我们监听了input的keydown事件，当输入 1 时移除了监听事件
移除监听事件是通过observer.complete()实现的。

### 介绍
Observable 目的就是将 DOM事件，计时器间隔和套接字等（model push-based data sources）可以用流的方式来处理
API可以看文档，这里再看一个复杂一些的例子。

```js
// 这个函数接受两个Observable实例，
// 用一个流去控制另一个的完成与否
function takeUntil(stream, control) {

  return new Observable(sink => {
      let source = stream.subscribe(sink);

      let input = control.subscribe({

          next: x => sink.complete(x),
          error: x => sink.error(x),
          complete: x => sink.complete(x),
      });

      return _=> {
          source.unsubscribe();
          input.unsubscribe();
      };
  });
}

// 将事件监听转变成 Observable
function listen(element, eventName) {
  return new Observable(sink => {
      function handler(event) { sink.next(event) }
      element.addEventListener(eventName, handler);

      return _=> {
        element.removeEventListener(eventName, handler)
      };
  });
}

// 鼠标拖拽的实现
function mouseDrags(element) {

  // For each mousedown, emit a nested stream of mouse move events which stops
  // when a mouseup event occurs
  return new Observable(sink => {
    listen(element, "mousedown").subscribe(e => {

      e.preventDefault();
      takeUntil(
          listen(element, "mousemove"),
          listen(document, "mouseup")).subscribe({ next(x) { sink.next(x); } });
    });
  })
}

// 订阅拖拽事件流
mouseDrags(document.body).subscribe({
  next(e) { console.log(`DRAG: <${ e.x }:${ e.y }>`) }
});
```

##总结
其实如果你仅仅使用了redux，那么你可能根本用不到 Observable。
其内部实现也是用过store.subscribe实现的
```js
// redux  createStore.js line 237-251
// 此处 outerSubscribe 就是 store.subscribe
// 然后将 observer.next的调用 放在了listener里调用
subscribe(observer) {
  if (typeof observer !== 'object' || observer === null) {
    throw new TypeError('Expected the observer to be an object.')
  }

  function observeState() {
    if (observer.next) {
      observer.next(getState())
    }
  }

  observeState()
  const unsubscribe = outerSubscribe(observeState)
  return { unsubscribe }
},
```
既然用不到，为什么redux为什么还要加上这一段呢？原因是为了配合一些响应式编程的库使用， 例如Rxjs。
在redux测试文件中 我们可以看到其用法
```js
const store = createStore(combineReducers({ foo, bar }))
const observable = Rx.Observable.from(store)
const results = []

const sub = observable
  .map(state => ({ fromRx: true, ...state }))
  .subscribe(state => results.push(state))

store.dispatch({ type: 'foo' })
sub.unsubscribe()
store.dispatch({ type: 'bar' })

expect(results).toEqual([
  { foo: 0, bar: 0, fromRx: true },
  { foo: 1, bar: 0, fromRx: true }
])
```
