# react-router 精读

## javascript 基础

### location 对象
- .hash: #号后跟零个或多个字符(包括#)
- .host: location.hostname + location.port
- .href: 完整url location.toString()也是这个值
- .search ?号后跟零个或多个字符(包括?),到url尾部 但不包括hash
- 位置操作 三种方法location.assign('xxxx'), location = 'xxxx', location.href = 'xxxx'
- 通过location修改URL后，浏览器的历史记录会增加一条记录。通过replace方法则不会.reload重载页面，reload(true) 强制从服务器重载

### history 对象
- .go(int | string) int：向前或向后，string：跳到包含该字符串的第一个位置。
- .back() .forward() 不解释
- .length 长度 用来判断是不是用户打开窗口的第一个页面 