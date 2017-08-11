### 通配符选择器
通配选择器（universal selector），显示为一个星号（*）。
该选择器可以与任何元素匹配，就像是一个通配符。

例如，下面的规则可以使文档中的每个元素都为红色：

```css
    *{color:red}
```


### 多类选择器
通过把两个类选择器链接在一起，仅可以选择同时包含这些类名的元素（类名的顺序不限）。

如果一个多类选择器包含类名列表中没有的一个类名，匹配就会失败。请看下面的规则：

```css
    .important.urgent {background:silver;}
```

这个选择器将只匹配 class 属性中同时包含词 important 和 urgent 的 p 元素。因此，如果一个 p 元素的 class 属性中只有词 important 和 warning，将不能匹配。不过，它能匹配以下元素：

```html
    <p class="important urgent warning">
        This paragraph is a very important and urgent warning.
    </p>
```

### 子元素选择器
如果只选择某个元素的子元素，请使用子元素选择器（Child selector）。
例如，如果您希望选择只作为 h1 元素子元素的 strong 元素，可以这样写：


```css
    h1 > strong {color:red;}
```
这个规则会把**第一个** h1 下面的两个 strong 元素变为红色，但是第二个 h1 中的 strong 不受影响

### 相邻兄弟选择器
相邻兄弟选择器可选择**紧接**在另一元素后的元素，且二者有**相同父元素**。
例如，如果要增加紧接在 h1 元素后出现的段落的上边距，可以这样写：


```css
    h1 + p {margin-top:50px;}
```

这个选择器读作：“选择紧接在 h1 元素后出现的段落，h1 和 p 元素拥有共同的父元素”。

* 注意：用一个结合符只能选择两个相邻兄弟中的第二个元素。请看下面的选择器：

```css
    li + li {font-weight:bold;}
```
上面这个选择器只会把列表中的第二个和第三个列表项变为粗体。第一个列表项不受影响。

### 伪类

属性描述 | header 2
---|---
:active       |  向被激活的元素添加样式
:focus        |  向拥有键盘输入焦点的元素添加样式
:hover        |  当鼠标悬浮在元素上方时，向元素添加样式
:link         |  向未被访问的链接添加样式
:visited      |  向已被访问的链接添加样式
:first-child  |  向元素的第一个子元素添加样式
:lang         |  向带有指定 lang 属性的元素添加样式。
