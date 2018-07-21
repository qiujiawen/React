# Sass语法

Sass是一个CSS的扩展，它在CSS语法的基础上，允许使用变量、嵌套规则、混合、导入等功能，令CSS更加强大与优雅。使用Sass以及Compass样式库有助于更好地组织管理样式文件，以及更高效地开发项目。

## 基本用法

#### 变量

允许使用变量，所有变量以$开头

```
$blue : #1875e7;　
div {
　color : $blue;
}
```

>如果变量需要镶嵌在字符串之中，就必须需要写在#{}之中

```
$side : left;
.rounded {
　　　　border-#{$side}-radius: 5px;
　　}
```

#### 计算功能

允许在代码中使用算式：

```
　　body {
　　　　margin: (14px/2);
　　　　top: 50px + 100px;
　　　　right: $var * 10%;
　　}
```

#### 嵌套

允许选择器嵌套

```
　　div h1 {
　　　　color : red;
　　}
```

上面例子改为嵌套的方式

```
　　div {
　　　　hi {
　　　　　　color:red;
　　　　}
　　}
```

>属性可以嵌套

比如border-color属性

```
　　p {
　　　　border: {
　　　　　　color: red;
　　　　}
　　}
```

>在嵌套的代码块内，可以使用&引用父元素

比如a:hover伪类

```
a {
　　　　&:hover { color: #ffb3ff; }
　　}
```

#### 注释

标准的CSS注释 /* comment */ ，会保留到编译后的文件。

单行注释 // comment，只保留在SASS源文件中，编译后被省略。

在/*后面加一个感叹号，表示这是"重要注释"。即使是压缩模式编译，也会保留这行注释，通常可以用于声明版权信息。
```
/*!
　　　　重要注释！
　　*/
```

#### 代码的重用-继承

允许一个选择器，继承另一个选择器

```
.class1 {
　　　　border: 1px solid #ddd;
　　}
```

class2要继承class1，就要使用@extend命令：

```
.class2 {
　　　　@extend .class1;
　　　　font-size:120%;
　　}
```

#### 代码的重用-Mixin

Mixin有点像C语言的宏（macro），是可以重用的代码块。

使用@mixin命令，定义一个代码块

```
@mixin left {
　　　　float: left;
　　　　margin-left: 10px;
　　}
```

使用@include命令，调用这个mixin

```
div {
　　　　@include left;
　　}
```

> mixin可以指定参数和缺省值

```
@mixin left($value: 10px) {
　　　　float: left;
　　　　margin-right: $value;
　　}
```

使用的时候，根据需要加入参数

```
div {
　　　　@include left(20px);
　　}
```

#### 代码的重用-插入文件

@import命令，用来插入外部文件

```
@import "style.css";
```

## 高级用法

#### @if

@if 指令需要一个SassScript表达和嵌套在它下面要使用的样式，如果表达式返回值不为 false 或者 null ，那么后面花括号中的内容就会返回

```
p {
  @if 1 + 1 == 2 { border: 1px solid;  }
  @if 5 < 3      { border: 2px dotted; }
  @if null       { border: 3px double; }
}
```

编译后
```
p {
  border: 1px solid;
}
```

>@if 语句后面可以跟多个@else if语句和一个 @else 语句。 如果@if语句失败，Sass 将逐条尝试@else if 语句，直到有一个成功，或如果全部失败，那么会执行@else语句。 例如：

```
$type: monster;
    p {
      @if $type == ocean {
        color: blue;
      } @else if $type == matador {
        color: red;
      } @else if $type == monster {
    color: green;
  } @else {
    color: black;
  }
}
```

编译后
```
p {
  color: green; }
```


#### @for

@for指令重复输出一组样式。

>@for $var from {start} through {end}

$var可以是任何变量名；from ... through的形式，范围包括 start 和 end 的值

>@for $var from {start} to {end}

$var可以是任何变量名；from ... to的形式从 start 开始运行，但不包括 end 的值

```
@for $i from 1 through 3 {
      .item-#{$i} { width: 2em * $i; }
}
```

编译后：

```
.item-1 {
  width: 2em; }
.item-2 {
  width: 4em; }
.item-3 {
  width: 6em; }
```

#### @each

>@each $var in {list or map}

$var可以是任何变量名，{list or map} 是一个返回列表 list 或 map 的 SassScript 表达式；@each 规则将$var设置为列表（list）或map中的每个项目，输出样式中包含使用$var的值。

```
@each $animal in puma, sea-slug, egret, salamander {
      .#{$animal}-icon {
        background-image: url('/images/#{$animal}.png');
  }
}
```

编译后：

```
.puma-icon {
  background-image: url('/images/puma.png'); }
.sea-slug-icon {
  background-image: url('/images/sea-slug.png'); }
.egret-icon {
  background-image: url('/images/egret.png'); }
.salamander-icon {
  background-image: url('/images/salamander.png'); }
```

参考文档：http://www.css88.com/doc/sass/
