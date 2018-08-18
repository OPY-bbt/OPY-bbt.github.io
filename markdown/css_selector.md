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
### ::after（最后一个子元素）
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
### ::before（第一个子元素）
### ::cue (Web Video Text Tracks)
### ::first-letter
- 如果标点符号开头，那么标点符号与第一个非标点符号会被选中
- 如果元素有before伪类，且content为字符，则content会被认为开头
### ::first-line
### ::selection (选中被用户高亮的部分，例如 选中文本)
### ::slotted （web component 相关）
### ::backdrop （全屏元素之下的第一个元素）
### ::placeholder （placeholder text ）
### ::marker (选中list-item前面的标志，不过没有浏览器支持)
### ::spelling-error（没有浏览器支持)
### ::grammar-error（没有浏览器支持)

## 伪类（Pseudo-class）
### :active | element is being activated by the user
### :matches | select any element in list
```css
:matches(header, main, footer) p:hover {
  color: red;
  cursor: pointer;
}
header p:hover,
main p:hover,
footer p:hover {
  color: red;
  cursor: pointer;
}
```
### :any-link | matches \<a>, \<area>, or \<link> element that has an href attribute
### :checked | radio, checkbox, option element that is checked
### :default | default element in form 
### :defined | element built in to the browser, and custom elements that has successfully defined
### :dir | matches elements based on the directionality of the text contained in them
### :disabled | disabled element
### :empty | element has no children which is contains element node or text
### :enabled
### :first | the first page of a printed document.
### :first-child | first element among a group of sibling elements.
### :last-child | last element among a group fo sibling elements.
### :first-of-type | first element of its type among a sibling elements.
### :last-of-type | last element of its type among a sibling elements.
```html
<h2>Heading</h2>
<p>will be red</p>
<p>Paragraph 2</p>

p:first-of-type {
  color: red;
  font-style: italic;
}

<!-- note that the universal selector(*) is implied when no simple selector is written -->
<!-- p :first-of-type === p: *:first-of-type -->
```
### :fullscreen | represents an element that's displayed when the browser is in fullscreen mode
### :focus | element has received focus
### :indeterminate | form element whose state is indeterminate.
### :in-range | input element whose current value is within the range limits specified by the min and max attributes
### :out-of-range
### :invalid | input and other form element whose contents fail to validate.
### :lang | element based on the language they are determined to be in.
### :left | all left-hand pages of a printed document
### :right
### :link | element has not yet been visited
### :not | do not match selector
### :nth-child | keyword values(odd, even), functional notation(An+B, n begin at 0)
### :nth-last-child | counting from the end.eg: nth-last-child(-n + 3) last three elements
### :nth-of-type
### :nth-last-of-type
### :only-child | only child of its parent
### :only-of-type | represents an element that has no siblings of the same type.
### :optional | input select textarea element that don't have the required attritude set on it
### :required
### :read-only | element that cannot be edit by user
```html
<p>selected</p>
<input readonly />
<p contenteditable="true">not selected<p>
```
### :read-write | element that can
### :root | html
### :scope | use style scope attritude to define a new scope, if not use, the root is html
### :target | element with an id matching url's fragment
### :valid | form element validate successfully
### :visited | link that the user already visited


