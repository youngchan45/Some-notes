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

   ```
   .item{flex:0;}	//为0
   .item{
   	flex-grow: 0;	//占剩余空间中总和的3份
   	flex-shrink: 0;		//收缩3份
   	flex-basis: 0%/0px；	// 有内容，则靠内容撑开；
   						无内容，无显示
   ps：flex-basis不要直接写0，应带单位。当直接写0时，浏览器会解析为0px（虽然测试后发现解析为0px和0%的效果是一样的，但是以防万一）
   ```

5. 为一个长度或百分比时

   ```
   .item{flex: 0%;}	//0%是一个百分比而不是一个非负数字
   .item{
   	flex-grow: 1;	//如4所述，除非内有内容，不然都为0的话是不显示的，所以这里是不为0的，不然写多这些代码就没有意义了	
   	flex-shrink: 1;		//同上
   	flex-basis: 0%;		//由伸缩值撑开
   }
   ```

6. 取值为两个非负数字

   ```
   .item{flex: 2 3;}
   .item{
   	flex-grow: 2;	//桉顺序赋值
   	flex-shrink: 3;
   	flex-basis: 0%;
   }
   ```

7. 一个非负数字和一个百分比/长度

   ```
   .item{flex: 2 30px;}
   .item{
   	flex-grow: 2;	//数字赋值给伸展
   	flex-shrink: 1;	  //收缩值还是保持默认的1，即在空间不足时自动缩小
   	flex-basis: 30px;
   }
   ```

   **总结：有数字就给伸缩项，因为它们没有单位；长度或百分比就给basisi（主要成分），因为它带单位**。

