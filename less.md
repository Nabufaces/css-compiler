## less

#### NPM安装less：
```bash
    npm install -g less
```
lessc  命令，你可以通过类似下面这个命令来编译你的.less文件来输出一个CSS文件
```bash
    lessc styles.less > styles.css
```

#### 1. Variables（变量）

* 定义 @variable: value

```less
    @background-color: #ffffff;
    @text-color: #1A237E;
    
    p{
      background-color: @background-color;
      color: @text-color;
      padding: 15px;
    }
```

#### 2. Mixins

2.1 LESS可以帮你把一个已经存在的class或者id下面所有的样式应用在其他的选择器里：
```less
    #circle{
      background-color: #4CAF50;
      border-radius: 100%;
    }
    #small-circle{
      width: 50px;
      height: 50px;
      #circle
    }
```

2.2 如果不想把用于mixin输出在最终的css文件里，你可以加上一个圆括号：
```less
    #circle(){
      background-color: #4CAF50;
      border-radius: 100%;
    }
    #small-circle{
      width: 50px;
      height: 50px;
      #circle
    }
```

2.3 mixins可以接收参数：
```less
    #circle(@size: 25px){
      background-color: #4CAF50;
      border-radius: 100%;
      width: @size;
      height: @size;
    }
    #small-circle{
      #circle
    }
    #big-circle{
      #circle(100px)
    }
    /* 我们在circles添加了一个参数用于width和height，同时设置默认值为25px。
    下面会生成一个宽高为25的#small-circle和一个宽高为100的#big-circle。*/
```

#### 3. Nesting and Scope(嵌套和作用域)
嵌套可以帮助你将html的页面结构和样式表的结构相同，同时也可以减少冲突。
```less
    ul{
      background-color: #03A9F4;
      padding: 10px;
      li{
        background-color: #fff;
        margin: 10px 0;
      }
    }
```

编译后的css：
```css
    ul {
      background-color: #03A9F4;
      padding: 10px;
    }
    ul li {
      background-color: #fff;
      margin: 10px 0;
    }
```

* 和在编程语言中一样，Less中变量的使用也是依赖于作用域。如果一个变量没有
在当前的作用域被定义，那么LESS会一直向上找，并使用最靠近的声明的变量。

例：
```less
    @text-color: #000000;
    
    ul{
      @text-color: #ffffff;
      background-color: #03A9F4;
      li{
        color: @text-color;
      }
    }
```

编译后的css：
```css
    ul {
      background-color: #03A9F4;
    }
    ul li {
      color: #ffffff;
    }
```


#### 4. Operations

```less
    @div-width: 100px;
    @color: #03A9F4;
    
    #left{
      width: @div-width;
      background-color: @color - 100;
    }
    #right{
      width: @div-width * 2;
      background-color: @color;
    }
```



#### 5. Functions

```less
    @var: #004590;
    
    div{
      background-color: @var;
      &:hover{
        background-color: fadeout(@var, 50%)
      }
    }
```

编译后的css：
```css
    div {
      background-color: #004590;
    }
    div:hover {
      background-color: rgba(0, 69, 144, 0.5);
    }

```

```less
    span {  
        height:floor(2.8)px;  
    }  

```

```css
    span {
        height: 2px;
    }

```