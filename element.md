#### input验证数字类型

```html
方法一：
<el-input v-model.number="money">
	<template solt="append">元</template>
</el-input>
```

```javascript
方法二:
rules:{
peopleNum:[
	{
        type:"number",
        message:"请输入数字",
        trigger:"blur",
        transform:value=>Number(value)
	}
]
}
```

