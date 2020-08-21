WxValidate

下载地址：https://github.com/skyvow/wx-extend

使用文档：https://github.com/skyvow/wx-extend/blob/master/docs/components/validate.md

插件位置：[wx-extend](https://github.com/skyvow/wx-extend)/[src](https://github.com/skyvow/wx-extend/tree/master/src)/[assets](https://github.com/skyvow/wx-extend/tree/master/src/assets)/[plugins](https://github.com/skyvow/wx-extend/tree/master/src/assets/plugins)/**wx-validate**/WxValidate.js

1. 将WxValidate.js拷贝到小程序项目utils文件夹里

2. 可局部引入到所需的js文件里

   ```javascript
   import WxValidate from '../../utils/WxValidate.js'
   ```

3. 将需要提交的表单用<form bindsubmit="submitForm"></form>包裹起来（记得连埋提交按钮<button form-type="submit">一起）

4. 给每个表单项增加验证兼数据绑定name="phone"，name='content' 并在js文件的data里面添加绑定

5. 重点：添加四个函数：1）验证规则和报错规则的函数  2）调用验证函数并把报错规则传给3)的函数  3）报错时显示方式的函数  4）提交表单的函数  

   ```javascript
   //1）验证规则和报错规则的函数
   initValidate(){
       this.WxValidate=new WxValidate(rules,messages)
       //验证规则
       const rules={
           phone:{
               required:true,//是否必填
               tel:true//插件自带验证手机号码
           },
           content:{
               required:true,//是否必填
               minlength:2//最小长度
           }
       }
       //报错规则:当输入的数据不符合要求，显示的文案，不传则显示默认
       const message={
           //rules对象中的属性一 一对应
           phone:{
               required:"请输入手机号",
               tel:"手机号不符合规范"
           }，
           content:{
           	required:"请输入内容",
           	minlength:"至少两个字符"
       	}
       }
   }
   
   //2）调用验证函数并把报错规则传给3)的函数
   submitForm(e){//form标签绑定的事件
       console.log("获取",e.detail.value)//获取表单输入的数据对象
       if(!this.WxValidate.checkForm(e.deatil.value)){
           console.log("错误",this.WxValidate.errorList)//整个表单的错误列表对象
           const err=this.WxValidate.errorList[0]//每次报错都拿最新一条错误
           this.showToast(err)
           return false
       }
       //如果验证通过，则提交订单
       this.submitInfo(e.detail.value)
   }
   
   //3）报错显示函数
   showToast(err){
       wx.showToast({
         title: err.msg,//注意进入msg
         icon: "none"
       })
   }
   
   //提交表单的函数
   submitInfo(params){
       console.log("要提交的表单",params)
   }
   ```

   

6. 在onLoad中加入验证规则函数（一定要加，否则编译会报checkform is not a function）

   ```javascript
   onLoad：function(){
   	this.initValidate() //验证规则函数
   }
   ```

自定义规则函数