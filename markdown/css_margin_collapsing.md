# css margin collapsing
`今天正在开心的写些页面，写完发现。咦，为啥页面能上下滚动一些距离，明明页面中的元素没有超出100vh啊。慢慢排查，才发现是因为我给页面最外层div加了一个min-height:100vh，用于填充背景色，然后给div中最后一个元素设置了margin-bottom。当时才恍然大悟，原来min-height也可以触发margin collapsing。现在就学习一下还有哪些因素可以导致margin collapsing。ps：当时 firefox 倒是没有发现问题`

## 条件
- 块级元素的上下外边距，注意浮动或绝对定位的元素不会发生。
- 相邻的兄弟元素之间(除非后一个元素清除了浮动)
- 父级元素与第一个子元素或最后一个子元素之间
- - 如果没有 border，padding，inline part，bfc创建（bfc 会稍后讲），清除, 去分开父级元素的margin-top与第一个子元素的margin-top
- - 如果没有 border，padding，inline content, height, max-height, min-height，区分开父级元素的margin-bottom与最后一个子元素的margin-bottom
- - 空块元素 no border, padding, inline content, height, or min-height， 去分开marign-top 和 margin-bottom, margin-top 和 bottom之前将会发生坍塌。

## block formatting context (bfc)
`A block formatting context is a part of a visual CSS rendering of a Web page. It is the region in which the layout of block boxes occurs and in which floats interact with other elements.`
- the root element or something that contains it ？？？ `<body>`
- floats (elements where float is not none)
- absolutely positioned elements (elements where position is absolute or fixed)
- inline-blocks (elements with display: inline-block)
- table cells (elements with display: table-cell, which is the default for HTML table cells)
- block elements where overflow has a value other than visible
- flex items (direct children of the element with display: flex or inline-flex)
- grid items (direct children of the element with display: grid or inline-grid)

notice:
* Margin collapsing also occurs only between blocks that belong to the same block formatting context.