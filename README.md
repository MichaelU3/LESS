# LESS

Source: <http://www.bootcss.com/p/lesscss/>

## Features

### 变量

> @color: #44D926F   
> @light-blue: @nice-blue + #111;  
>    
>  #header{  
>    color: @light-blue;    
>  }  

### 混合

* 定义一些通用的属性集为一个class，然后在另一个class中去调用这些属性.
* 可以像函数一样定义带参数的属性集合
* @arguments 变量包含了所有传递进来的参数
* 如果想隐藏这个属性集合,定义不带参数属性集合

> //CSS 中不可见  
> .marging-base(){marging: 10px;}
>
> //CSS 中可见  
>.padding-base(@top: 0, @right: 15px, @bottom: 0, @left: 15px){  
>     padding: @arguments;  
>}  
>  \#header{  
>    .padding-base();   
>  }  
>  \#footer{  
>    .padding-base(10px, 20px, 10px, 20px);   
>  }  

### 模式匹配和导引表达式

#### 模式匹配

类似多态,根据参数的不同调用不同的属性

例: 想根据传入的参数来改变混合的默认呈现

>  
>.mixin (dark, @color) {  
>  color: darken(@color, 10%);  
>}  
>.mixin (light, @color) {  
>  color: lighten(@color, 10%);  
>}  
> // @_匹配任意值  
>.mixin (@_, @color) {  
>  display: block;  
>}  
>@swith: dark;  
>.class {  
>  .mixin(@switch, #888);  
>}  

#### 导引

当我们想根据表达式进行匹配，而非根据值和参数匹配时，导引就显得非常有用

when关键字用以定义一个导引序列

>.mixin (@a) when (lightness(@a) >= 50%) {  
>  background-color: black;  
>}  
>.mixin (@a) when (lightness(@a) < 50%) {  
>  background-color: white;  
>}  
>.mixin (@a) {  
>  color: @a;  
>}  

* 导引中可用的全部比较运算有： > >= = =< <
* 关键字true只表示布尔真值, 除去关键字true以外的值都被视示布尔假
* 条件表达式同样支持 AND 和 OR(使用",") 以及 NOT 来组合条件表达式
> .mixin (@a) when (@a > 10), (@a < -10) { ... }
> .math (@a) when (@a > 10) and (@a < 20) { background-color: red;}  
> .math (@a) when not (@a = 10)  { background-color: yellow; } 
* 导引可以无参数，也可以对参数进行比较运算
> .max (@a, @b) when (@a > @b) { width: @a }    
> .max (@a, @b) when (@a < @b) { width: @b }  
* 想基于值的类型进行匹配，我们就可以使用is*函式
  - iscolor
  - isnumber
  - isstring
  - iskeyword
  - isurl
  - ispixel
  - ispercentage
  - isem

### 嵌套规则

将后代选择器对应的属性直接定义在父类属性中

> #header {  
>  color: black;  
>  //使用&串联选择器
>  &:hover {}
>  .navigation {  
>    font-size: 12px;  
>  }  
>  .logo {  
>    width: 300px;  
>    &:hover { text-decoration: none }  
>  }  
> }  

### 命名空间

了更好组织CSS或者单纯是为了更好的封装，将一些变量或者混合模块打包起来


> \#bundle {  
>  .button () {  
>    display: block;  
>    border: 1px solid black;  
>    background-color: grey;  
>    &:hover { background-color: white }  
>  }  
>  .tab { ... }  
>  .citation { ... }  
>}  
>\#header a {  
>  color: orange;  
>  #bundle > .button;  
>}  

### 字符串插值

@{name}这样的结构进行字符串赋值

> @base-url: "http://assets.fnord.com";  
> background-image: url("@{base-url}/images/bg.png");

### 避免编译

输出一些不正确的CSS语法或者使用一些 LESS不认识的专有语法,可以在字符串前加上一个 ~, 将要避免编译的值用 “”包含起来

> .class {  
>   filter: ~"ms:alwaysHasItsOwnSyntax.For.Stuff()";  
> }  

### JavaScript 表达式

可以通过反引号的方式使用JavaScript 表达式

> @var: `"hello".toUpperCase() + '!'`;  
> 输出  
> @var: "HELLO!";  


