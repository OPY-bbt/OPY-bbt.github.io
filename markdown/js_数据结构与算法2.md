# 队列
`与栈不同之处在于先进先出`

# 链表
`通过每个节点储存前后节点的引用，形成的链表结构，避免了在数组中因添加删除元素导致移动元素而造成的性能问题`

## 双向链表
`单向链表只能从head到tail单项遍历，而双向链表通过在每个节点存储前后节点的引用，可以双向遍历`

## 循环链表
`与单向链表不同之处在于其链表尾部并不是指向null，而是指向head`

## 双向循环链表
`双向链表 + 循环链表`

# 集合
`无序， 唯一`

# 字典
`Dictionary 以[键，值]存储`

# 散列表 HashMap
`散列算法的作用是 尽可能快的在数据结构中找到一个值。`

```js
function HashTable() {
    var table = [];
}
// 散列函数
var loseloseHashCode = function(key) {
    var hash = 0;
    for (var i = 0; i < key.length; i++) {
        hash += key.charCodeAt(i);
    }
    return hash % 37;
}
this.put = function(key, val) {
    var position = loseloseHashCode(key);
    table[position] = val;
}
// 这样的好处是什么，干脆用字典得了，没了解散列的适用场景。。。。。
```

## 处理冲突的3种方法
- 分离链接      
`散列 + 链表。没错就是在散列的每一个位置创建一个链表。。。`
- 线性探查     
`如果index的位置被占据了，尝试index+1，以此类推 `
- 双散列法     
`于线性探查基本一致，唯一不同是：它不是检查冲突位置后的每一个位置，而是采用另一个散列函数产生一个固定的增量。（跳跃式检查和插入，减小聚焦大小）`