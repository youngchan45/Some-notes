### CSS备忘

- rem 单位翻译为像素值是由 html 元素的字体大小决定的。 此字体大小会被浏览器中字体大小的设置影响，除非显式重写一个具体单位。

- em 单位转为像素值，取决于他们使用的字体大小。 此字体大小受从父元素继承过来的字体大小，除非显式重写与一个具体单位。

- 1in（英寸）等于96px

- outline和border的区别：

  ```css
  1. border可用于几乎所有有形的html元素，而outline是针对连接、表单空间和ImageMap等元素设计
  2. outline的效果随focus而自动出现，相应地随着blur（失去焦点）而自动消失
  3. outline不会像border那样影响元素的尺寸或位置
     ps：去除焦点虚线：style= "outline: medium none;" hidefocus="true";
  ```

  

### flex缩写如何记住

1. ```css
   .item{flex:none;}	//大家一起没有
   .item{
   	flex-grow: 0;	//即使父元素有剩余空间，也不去索取
   	flex-shrink: 0;		//即使父元素有剩余空间，也不去索取
   	flex-basis: auto;	//1.如果width有非auto的值，则取width/height的值（取决direction）；
       						2.没有设置width/height，则向上看content，content是否有非auto值
       						3.都没有则根据flex-basis内的内容撑开
   }
   ```

2. ```css
   .item{flex: auto;}	//自动填满剩余空间
   item{
   	flex-grow: 1;	//自动填满剩余空间
   	flex-shrink: 1;		//当空间不足时，自动缩小填满剩余空间
   	flex-basis: auto;	//1.如果width有非auto的值，则取width/height的值（取决direction）；
       					  2.没有设置width/height，则向上看content，content是否有非auto值
       					  3.都没有则根据flex-basis内的内容撑开
   }
   ```

3. 区别于2，这三个值的默认值分别为 0，1，auto

   ```css
   .item{flex:initial}		//默认
   .item{
   	flex-grow: 0;	//即使父元素有剩余空间，也不去索取
   	flex-shrink: 1;		//当空间不足时，自动缩小填满剩余空间
   	flex-basis: auto;	//1.如果width有非auto的值，则取width/height的值（取决direction）；    			 2.没有设置width/height，则向上看content，content是否有非auto的值
       					  3.都没有则根据flex-basis内的内容撑开
   }
   ```

4. 当flex为一个非负数字时，要分情况

   ```Css
   .item{flex:3;}	//非0
   .item{
   	flex-grow: 3;	//占剩余空间中总和的3份
   	flex-shrink: 3;		//收缩3份
   	flex-basis: 0%/0px；	// 有无内容都可以，由伸缩值撑开
   }
   ```

   ```css
   .item{flex:0;}	//为0
   .item{
   	flex-grow: 0;	//占剩余空间中总和的3份
   	flex-shrink: 0;		//收缩3份
   	flex-basis: 0%/0px；	// 有内容，则靠内容撑开；
   						无内容，无显示
   ps：flex-basis不要直接写0，应带单位。当直接写0时，浏览器会解析为0px（虽然测试后发现解析为0px和0%的效果是一样的，但是以防万一）
   ```

5. 为一个长度或百分比时

   ```css
   .item{flex: 0%;}	//0%是一个百分比而不是一个非负数字
   .item{
   	flex-grow: 1;	//如4所述，除非内有内容，不然都为0的话是不显示的，所以这里是不为0的，不然写多这些代码就没有意义了	
   	flex-shrink: 1;		//同上
   	flex-basis: 0%;		//由伸缩值撑开
   }
   ```

   

6. 取值为两个非负数字

   ```css
   .item{flex: 2 3;}
   .item{
   	flex-grow: 2;	//桉顺序赋值
   	flex-shrink: 3;
   	flex-basis: 0%;
   }
   ```

7. 一个非负数字和一个百分比/长度

   ```css
   .item{flex: 2 30px;}
   .item{
   	flex-grow: 2;	//数字赋值给伸展
   	flex-shrink: 1;	  //收缩值还是保持默认的1，即在空间不足时自动缩小
   	flex-basis: 30px;
   }
   ```

   **总结：有数字就给伸缩项，因为它们没有单位；长度或百分比就给basisi（主要成分），因为它带单位**。

- flex-grow: 值只能为数字，不能为单位和百分数
  flex-shrink：值只能为数字，不能为单位和百分数
  flex- basis： 值不能为数字，可以为单位和百分数

- #### flex 匹配规则

  1. 如果为3个值的话，必须2个是数字，一个是单位或者百分数，顺序可以改变
  2. 匹配优先级：flex-grow > flex-shrink > flex- basis

- **兼容性处理：**

安卓4.4以下的手机不支持flex布局。所以要做好兼容工作。

注意在写flex的时候加上：

```css
  display: -webkit-box;
  display: -moz-box;
  display: -webkit-flex;
  display: -ms-flexbox;
  display: flex;
```

### grid布局

- place-content属性是align-content和justify-content的合并简写形式

  ```css
  place-content: <align-content> <justify-content>;  //先列后行
  ```

- 设置位置的格式

  1. 明确表示第几行开始，第几行结束

     ```css
     item{
     	grid-column: 1 / 3;	  //<grid-column-start> <grid-column-end>
     	grid-row: 1 / 2;	//<grid-row-start> <grid-row-end>
     }
     ```

  2. 表示从第几行开始，跨越几个网格（结束）

     ```css
     item{
     	grid-column: 1 / span 2;    //等同于 1 / 3
     	grid-row: 1;    //斜杠和后面的部分可省略，默认跨越一个网格
     }
     ```

  3. 直接表示跨越几个网格

     ```css
     item1{
     	grid-column-start: span 2;  //一般有指定说从哪条开始，没有的话则默认布局
     }
     
     item2{
     	grid-column-start：2；
     	grid-column-end： span3； //一般有指定说从哪条开始，没有的话则默认布局
     }
     ```

  4. 表示从第几行开始，碰到哪行时就结束

     ```
     占坑，目前找不到资料
     ```

  5. 明确表示在哪个区域

     ```css
     item{
     	grid-area：e；
     }
     //或者
     item{
     	grid-area：1 / 1 / 3 / 3    // <row-start> / <column-start> / <row-end> / <column-end>先一起设置开头，再一起设置结尾
     }
     ```

- 设置每个网格所占的行和列，一对引号表示一行，哪怕所属的列不同

  `细节`： 1. 每行结尾不需要加分号（最后要加）

  ​			 2. 值之间也不需要加逗号

  ​			 3. 行、列、区域属性值单词后面要加 s 

  ```css
  错误：
  container{
  	display: grid;
      grid-complate-rows: 1fr 5fr 1fr;     //易错点 当设置rows行时，实际设置的是视觉上的高
      grid-complate-columns: 2fr 5fr 3fr;		// 当设置columns，实际设置的是视觉上的宽度
  	grid-template-areas:
  	"header header header" 
  	"nav" "main" "nav"
  	"footer footer footer";
  }
  
  正确：
  grid-template-areas:
  	"header header header" 
  	"nav main nav"
  	"footer footer footer";
  grid-gap: .75em;    //不写前面的0，节省字节
  nav{
      grid-area: nav;   //给区域起名
  }
  ```

- auto-fit 和 auto-fill 区别

  先行知识点：

  ```css
  grid-template-column: repeat(auto-fit,minmax(200px,1fr) );    // 这个格式看起来虽然复杂，但和函数repeat()一样的，auto-fit为重复的数量，minmax(200px,1fr)为重复的值
  给定一个元素的最小宽度，该元素可以根据窗口的大小来显示该元素的加宽和变窄，在小于最小宽度时换行
  ```

- 相同点：


    1. 尽可能地创建轨道
  2. 剩下的空间要是不足一个轨道，则把这个轨道平均分配给已有的轨道

- 不同点：


  当视口的宽度大到能够容纳额外列时，才可以看出差别；

1. 如果视口宽度过小，则两者都需要换行，看不出区别：

auto-fill:

  auto-fit:

  auto-fit:

2. 当一行的宽度足够，两者都是随着宽度增大而增加列数，区别在于：

  auto-fill尸位素餐，尽可能地容纳更多的列，即使是空的：

  auto-fit则用已有的列去扩张占满一行里剩下的空间，每列增加的宽度为剩下的空间去平均分给它们：

  auto-fit则用已有的列去扩张占满一行里剩下的空间，每列增加的宽度为剩下的空间去平均分给它们：

`flex-shrink`默认为1，也就是当不够分配时，元素都将等比例缩小，占满整个宽度

清除浮动：当父元素没有设置宽或高，又需要由子元素来撑开父元素时，需要清除由于子元素浮动带来的父元素高度坍塌的影响

display:inline-block 只能设置由左往右排列

从XHTML文档有意义性及用户体验角度来说，strong逻辑标签比b标签更加合适

#### css规范

1.一律小写；

2.尽量用英文；

3.尽量不加中杠和下划线；

4.尽量不缩写，除非一看就明白的单词，如：wrapper可以写成wrap。



浏览器的默认的font-size是16px

chrome最小字体为12px

#### scss四个常用应用：

```npm
npm install node-sass --save-dev
npm install sass-loader --save-dev
```

###### 1. 变量

```scss
$app-background:#d00;
#app{
    margin:auto;
    background:$app-background;
}
```

###### 2.@mixin混合器

```scss
如果存在大段可以重复使用的css代码，可以使用混合器方便地调用它
建立一个圆角功能的混合器：
@mixin rounded-corners{
    border-radius:5px;
    -moz-border-radius:5px;
    -webkit-border-radius:5px;
}
使用@include来引用这个混合器
.notice{
    background:green;
    @include rounded-corners;
}
编译后：
.notice{
    background:green;
    border-radius:5px;
    -moz-border-radius:5px;
    -webkit-border-radius:5px;
}

混合器一个很重要的特性就是可以传递参数
@mixin makeradius($radius){
    border-radius:$radius;
}
.test{
    background:blue;
    @include makeradius(8px);
}
编译后：
.test{
    background:blue;
    border-radius:8px;
}

设置参数默认值：
@mixin setborder($color,$width:2px){
    border:{
        color:$color;
        width:$width;
        style:dashed;
    }
}
p{
    @include setborder(green);
}
h1{
    @include setborder(green,4px)
}
编译后：
p{
    color:green;
    width:2px //没有传递参数，所以使用默认值
    style:dashed;
}
h1{
    color:green;
    width:4px; //有传递新参数，新参数覆盖默认参数
    style:dashed;
}
```

###### 3.@extend 继承

```scss
.test1{
    color:red;
}
.test2{
    @extend .test1;
    background:#999;
}
编译后：
.test1, .test2{
    color:red;
}
.test2{
    background:#999;
}
```

###### 4.属性嵌套

```scss
.loginbar{
    img{
        width:20px;
    }
}
编译后：
.loginbar img{
        width:20px;
}


如果要在嵌套的选择器里应用一个类似:hover的伪类，则需要用到&这个连接夫选择器的标识符：
.loginbar{
    div{
        color:red;
        &hover{
            color:blue;
        }
    }
}
编译后：
.loginbar div{color:red;}
.loginbar div:hover{color:blue;}
```

```html
如果使用<thead/>和<tfoot/>标签，建议使用如下次序使用它们： <thead/>、<tfoot/>、<tbody/>，浏览器会自动将<tfoot/>呈现在最下面
```

```css
* {  
-webkit-box-sizing: border-box;     
   -moz-box-sizing: border-box;          
		box-sizing: border-box; 
	}
//支持IE8+
```

```css
同一 规则下的属性在书写时，应按功能进行分组。并以 
Formatting Model（布局方式、位置） >
Box Model（尺寸） > 
Typographic（文本相关） > 
Visual（视觉效果） 
的顺序书写，以提高代码的可读性。
```

```css
- Formatting Model 相关属性包括：position / top / right / bottom / left / float / display / overflow 等
- Box Model 相关属性包括：border / margin / padding / width / height 等
- Typographic 相关属性包括：font / line-height / text-align / word-wrap 等
- Visual 相关属性包括：background / color / transition / list-style 等
```

#### 修改input里面的样式

```css
input::webkti-input-placeholder{
    color:#ccc;
}
```

