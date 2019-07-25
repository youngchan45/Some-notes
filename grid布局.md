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



-  设置每个网格所占的行和列，一对引号表示一行，哪怕所属的列不同

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
  
相同点：
  
  1. 尽可能地创建轨道
2. 剩下的空间要是不足一个轨道，则把这个轨道平均分配给已有的轨道
  
不同点：
  
  当视口的宽度大到能够容纳额外列时，才可以看出差别；如果视口宽度过小，则两者都需要换行，看不出区别：
  
auto-fill:
  
  ![img](https://github.com/youngchan45/Some-notes/blob/master/img/fill.png)
  
  auto-fit:
  
  ![1564034212673](https://github.com/youngchan45/Some-notes/blob/master/img/fit.png)
  
  当一行的宽度足够，两者都是随着宽度增大而增加列数，区别在于：
  
  auto-fill尸位素餐，尽可能地容纳更多的列，即使是空的：
  
  ![1564034449468](https://github.com/youngchan45/Some-notes/blob/master/img/%E4%B8%8D%E7%94%A8%E6%8D%A2%E8%A1%8C-fill.png)
  
  auto-fit则用已有的列去扩张占满一行里剩下的空间，每列增加的宽度为剩下的空间去平均分给它们：
  
  ![1564034637338](https://github.com/youngchan45/Some-notes/blob/master/img/%E4%B8%8D%E7%94%A8%E6%8D%A2%E8%A1%8C-fit.png)
  
  
  
  
