

小程序进入后台时，先触发页面的生命周期函数onHide，再触发小程序的生命周期函数onHide；（小程序生命周期后退场）

小程序启动显示或从后台进入前台时，先触发小程序的生命周期函数Onshow，再触发页面的生命周期函数onShow。（小程序生命周期先显示）

后台： 当用户点击左上角关闭（或者右上角退出），或者按了home键离开微信，小程序并没有直接销毁，而是进入了后台。

前台： 当再次进入微信或者再次打开小程序，又会从后台进入前台。

销毁：只有当小程序进入后台一定时间（目前是5分钟），或者系统资源占用过高，才会被真正的销毁。



#### 加载背景图片

背景图片无法通过WXSS插入本地资源图片

1. 使用网络图片
2. 将背景图片进行编码base64进行转换，格式background-image:url("转换后得到的编码文本")
3. 使用</image>标签，修改图片的z-index为-999

#### 按钮背景透明

```css
background: transparent;/*完全透明*/
opacity:0.7;/*整个按钮的不透明度，会影响到文字，0完全透明，1完全不透明*/
background:rgba(255,255,255,0.7);/*仅调节背景的色彩，并加上不透明度，此例为70%不透明的白色*/
```

#### 空格

```html
设置空格
<text>1 23</text>
<text decode>1&nbsp;23</text>
<text>1\t23</text>  \t为空格转义符
```

```html
设置空格的大小
<text space="nbsp">1 23(空格根据字体设置)</text>
<text space="ensp">1 23(空格为中文字符一半大小)</text>
<text space="emsp">1 23(空格为中文字符大小)</text>
```

#### 换行

```html
<view>自带换行
<text>123\n456</text> \n为换行转义符
```

#### 二维码扫码取值

一般会把跳转的页码和页面参数放到scene里，但需要使用decodeURIComponent 对字符串进行解密

```javascript
onLoad:function(options){
    var scene =decodeURIComponent(options.scene)
}
```

#### 原生请求 （与get不同）

```javascript
wx.request({
  url: 'http://127.0.0.1:8080/test',
  method: "POST",
  * //指定请求方式，默认get*
  data: {
    id: 2010140
  },
  header: {
    * //默认值'Content-Type': 'application/json'*
    'content-type': 'application/x-www-form-urlencoded' * //post*
  },
  success: function (res) {
    console.log(res.data)
  }
})
```

#### 保存照片授权

有三种情况：

```javascript
wx.getSetting({
            success(res) {
                if (res.authSetting['scope.writePhotosAlbum']) {
                    _this.saveImg();
                } else if (res.authSetting['scope.writePhotosAlbum'] === undefined) {
                    wx.authorize({
                        scope: 'scope.writePhotosAlbum',
                        success() {
                            _this.saveImg();
                        },
                        fail(){
                            wx.showToast({
                                title: '您没有授权，无法保存到相册',
                                icon: 'none'
                            })
                        }
                    })
                }else {
                    wx.openSetting({
                        success(res) {
                            if (res.authSetting['scope.writePhotosAlbum']) {
                                _this.saveImg();
                            }else{
                                wx.showToast({
                                    title:'您没有授权，无法保存到相册',
                                    icon:'none'
                                })
                                _this.setData({ isSaving: false });                                
                            }
                        }
                    })
                }
            }
        })
```

1.用户第一次使用，弹出授权



2.用户已经拒绝过授权

3.用户已经授权

#### 事件的继承和冒泡

- 使用bind开头的事件绑定，这种绑定不会阻止冒泡事件向上冒泡
- 使用catch开头的事件绑定，这种绑定可以阻止冒泡事件向上冒泡

`event`对象中的`target`是事件产生的源头组件，而`currentTarget`是当前捕获这个事件的组件。

`touches` 和 `changedTouches` 表示一个或多个手指在屏幕上的触摸位置和变动位置等信息，可以用来实现多点触摸的高级手势处理。

#### 可拖曳悬浮按钮

使用movable-area和movable-view，movable-view必须是movable-area的直接子元素

movable-area为可移动的区域，若为全屏移动的话，不需要获取屏幕的高度宽度，直接

```css
movable-area {
  height: 100%;
  width: 100%;
  position: fixed;//作为组件引入时
  left: 0;//只能设置left、top，设置right、bottom无效
  top: 0;
  pointer-events: none;//使用这个属性后，该元素就呈现一种可忽略的状态，点击会穿透它，直接点击到该元素下面的元素
  z-index: 999;
}
```

movable-view为里面移动的元素，直接把要移动元素的布局class类名写在movable-view即可，不需要在movable-view再嵌套一个盒子

```html
<movable-area class="area">
	<movable-view direction="all" scale="true" class="container service" bindtap="toService">
		<!-- <view class="service" bindtap="toService"> -->
		<image src="../../static/icon/service.png"></image>
		<view>联系客服</view>
		<!-- </view> -->
	</movable-view>
</movable-area>
```

这里有个坑，若需要设置按钮在初始化时在右下方且不随着页面滚动，不能用fixed

```css
movable-view {
  position: absolute;
  left:80%;//同样只能设置left、top
  top:86%;//百分比的自适应效果比rpx的好一点点
  z-index: 999;
  width: 110rpx;
  height: 110rpx;
  pointer-events: auto;//必写，才可以进行某些操作
}
```

#### 进入子页面后更新父页面的数据

```javascript
var pages =getCurrentPages();//获取当前页面栈
if(pages.length>1){
    var beforePage=pagesp[pages.length-2];//获取上一个页面实例对象
    beforePage.changeData();//触发父页面中的方法
}
```

```javascript
 wx.navigateBack({
    delta: 1,
    success: function (e) {
      var page = getCurrentPages().pop();
      if (page == undefined || page == null) return;
      page.onLoad();
    }
  })
```

#### getCurrentPages()的使用

getCurrentPages()函数用于获取当前页面栈的实例，以数组形式按栈的顺序给出，第一个元素为首页，最后一个元素为当前页面。
`注意：`
`1、不要尝试修改页面栈，会导致路由以及页面状态错误`
`2、不要在App.onLaunch的时候调用getCurrentPages()，此时page还未生成`

```javascript
let pages = getCurrentPages(); //当前页面栈 
//当前页面为页面栈的最后一个元素 
//首页为页面栈的第一个元素
let prevPage = pages[pages.length - 1];
//当前页面 or // pop() 方法用于删除并返回数组的最后一个元素 
let prevPage = pages.pop();
//当前页面  
console.log( prevPage.route) 
//举例：输出为‘pages/index/index’

常用场景：
1、利用页面栈的长度
例如：进入小程序非默认首页时，需要提供返回首页的按钮或者执行其他事件
onShow(){
    let pages=getCurrentPages();//当前页面栈
    if(pages.length==1){
        //todo
    }
}

2、跨页面赋值
let pages=getCurrentPages();//当前页面栈
let prevPage=pages[pages.length-2];//上一页面
prevPage.setData({
    //直接给上一页面赋值
})

```



#### 动态添加样式

```css
style = "opacity :{{num}}"
class = "{{data == 0 ? 'class1':'class2'}}"
```

#### 用setData修改对象的属性值

```javascript
data:{
    person:{
        width:10,
        color:yellow
    }
}

var color="person.color";
this.setData({
    [color]:blue//使用中括号【】将字符串包起来，为其赋值
})
```

#### 判断undefined(注意拼写 是undefined不是underfined)

```javascript
if(typeof(value)=="undefined"){
	console.log("undefined")
}
```

