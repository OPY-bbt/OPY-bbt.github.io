# css selector

## 基本选择器
- type element
- class .classname
- id #id
- \*
- attr 
- - [attr]            获得有此属性的元素
- - [attr=value]      获得有此属性并值相等的元素
- - [attr~=value]     获得有此属性并且value为以空格为间隔的元素此属性值之一
- - [attr|=value]     获得有此属性值并且其值为value或者以value和连字符-开头
- - [attr^=value]     获得此属性值为以value开头的元素
- - [attr$=value]     获得此属性值为以value结尾的元素
- - [attr*=value]     获得此属性值中含有value的元素
- - [attr operator i] 对于规则不区分大小写

## 组合选择器
- \+     紧邻兄弟选择器     即第二个节点紧邻着第一个节点，并且拥有共同的父节点
- ～    一般兄弟选择器      第二个节点在第一个节点后面的任意位置，并且这俩节点的父节点相同。
- \>     子选择器          直接子节点
- 空格   后代选择器         子代节点

## 伪元素（Pseudo-elements）
## ::after（最后一个子元素）
```html
  <!-- 用after元素创建tooltip -->
  <p>
    <span data-descr="collection of words and punctuation">text</span>
  </p>
  <style>
    span[data-descr] {
      position: relative;
      text-decoration: underline;
      color: #00F;
      cursor: help;
    }
    span[data-descr]:hover::after {
      content: attr(data-descr);
      position: absolute;
      left: 0;
      top: 24px;
      min-width: 200px;
      border: 1px #aaaaaa solid;
      border-radius: 10px;
      background-color: #ffffcc;
      padding: 12px;
      color: #000000;
      font-size: 14px;
      z-index: 1;
    }
  </style>
```
## ::before（第一个子元素）
## ::cue (Web Video Text Tracks)
## ::first-letter
- 如果标点符号开头，那么标点符号与第一个非标点符号会被选中
- 如果元素有before伪类，且content为字符，则content会被认为开头
## ::first-line
## ::selection (选中被用户高亮的部分，例如 选中文本)
## ::slotted （web component 相关）
## ::backdrop （全屏元素之下的第一个元素）
## ::placeholder （placeholder text ）
## ::marker (选中list-item前面的标志，不过没有浏览器支持)
## ::spelling-error（没有浏览器支持)
## ::grammar-error（没有浏览器支持)