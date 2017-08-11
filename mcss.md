## Mcss

#### 赋值操作
* mcss中的variable与以$开头
* mcss支持三种赋值^=,=与?=
    * ?=只在变量未赋值或null时生效,所有的值类型都可以被赋值,包括函数
    * ^=表示将赋值操作提升到全局作用域

```
    // $variable has scope
    $a = 10px;
    $a ?= 20px;
    
    body{
        left: $a;   // exports left: 10px;
    }
    
    // override before
    $a = 30px;
    
    body{
        left: $a;   // exports left: 30px;
    }
    
    // function is also a value can be assigned
    $fn ?= ($name) {
        left: $name;
    }
```


#### %符
* % 代表除最外层选择器之外的选择器序列，如：

```
    .ms-form
        input[type="range"],
        select{
          display: inline-block;
          .ms-form-stack %{
            display: block;
          }
        }
        // other ruleset
    }
```

编译后的css:

```css
    .ms-form input[type="range"],.ms-form select{
        display:inline-block;
    }
    .ms-form-stack  input[type="range"],.ms-form-stack  select{
        display:block;
    }
```

#### 函数

##### 1. 作为mixin混入使用
当function没有返回值时，函数成为一个mixin, 会将解释后的 function block输出，实现SCSS中的@include, 这也是最常用的方式


```
    // 带参数
    $size = ($width, $height){
        $height ?= $width;
        height: $height; 
        width: $width; 
    }
    // 不带参数, 可视为一个block模版  
    $clearfix = {
        *zoom: 1;
        &:before, &:after {
            display: table;
            content: "";
            line-height: 0; 
        }
        &:after {
            clear: both; 
        }
    }
    body{
        $clearfix();
        $size(5px);
    }
```

编译后的css:

```css
    body{
        *zoom:1;
        height:5px;
        width:5px;
    }
    body:before,body:after{
        display:table;
        content:"";
        line-height:0;
    }
    body:after{
        clear:both;
    }
```

##### 2. 作为函数使用
在解释function block时, 遇到了 @return 语句, 则中断返回. 注意返回值可以是另外一个函数. mcss函数本质上与mcss的javascript实现
的内建函数是一致的，优势是不需要树节点操作 。


```
    $abs = ($value){
        @if $value < 0 {
            @return -$value; }
        @return $value;
    }
    $min = (){
        $res = index($arguments, 0);
        @for $item of $arguments{
            @if $item < $res {
                $res = $item; }}
        @return $res;
    }
    @debug $min(1, 2, 3, 4, 0); // -> 0
    
    @debug $abs(-100px);   // 100px    
```

##### 3. transparent call
mcss支持类似 stylus 的transparent call (只适用于作为mixin使用的function)的调用方式


```
    $border-radius = ($args...){
        @if !len($args) { 
            error('$border-radius Requires at least one paramete')}
        $value = join($args, ' / ');
        -webkit-border-radius: $value;
           -moz-border-radius: $value;
                border-radius: $value;
    }
    body{
        $border-radius: 10px 20px;
        $border-radius: 10px 20px 100% , 20px; 
    }
```

编译后的css:

```css
    body{
      -webkit-border-radius:10px 20px;
      -moz-border-radius:10px 20px;
      border-radius:10px 20px;
      -webkit-border-radius:10px 20px 100% / 20px;
      -moz-border-radius:10px 20px 100% / 20px;
      border-radius:10px 20px 100% / 20px;
    }
```

##### 4. 参数
mcss支持丰富的参数类型: rest param 以及 default param 、named param;
```
    // 缺省值
    $default-param = ($left, $right = 30px ){
        default-param: $right;
    }
    // named param 一般用在大量default 只需要传入部分参数的情况下
    $named-param = ($color = 30px, $named){
        named: $named;
    }
    
    $rest-at-middle = ($left, $middle... , $right){
        rest-at-middle: $middle;
    }
    $rest-at-left = ($left... , $right){
        rest-at-left: $left;
    }
    $rest-at-right = ($left,$right...){
        rest-at-right: $right;
    }
    
    body{
        $named-param($named = 30px);
        $default-param(10px);
        $rest-at-middle(1, 2, 3, 4);
        $rest-at-left(1, 2, 3, 4);
        $rest-at-right(1, 2, 3, 4);
    }
```

编译后的css:

```css
    body{
      named: 30px;
      default-param:30px;
      rest-at-middle:2,3;
      rest-at-left:1,2,3;
      rest-at-right:2,3,4;
    }
```

* 注意rest param 不能有默认值, 在参数有named param时 这个参数会被从参数列表中剔除，剩余的参数再进行赋值


##### 5. $arguments以及其他
在进入function block时, mcss会在当前作用域定义一个变量叫$arguments(Type: valueslist), 代表传入的所有参数

mcss不支持类似arguments[0]下标操作, 不过你可以通过内建函数 args(0)来得到同样的效果

```
    $foo = {
      first: args(0);
      seconed: args(1);
      arguments: $arguments
    }
    body{
        $foo: 10px, 20px
    } 
```

编译后的css:

```css
    body{
      first:10px;
      seconed:20px;
      arguments:10px,20px;
    }
```
