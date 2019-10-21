### JS备忘

##### 函数

- ```javascript
  var abs = function(x){
  	if(x >= 0){
  		return x;
  	} else {
  		return -x;
  	}
  };		//匿名函数赋值给变量abs，按照完整语法，需要在函数体末尾加一个分号 ；，表示赋值语句结束 
  ```

  #### 路径：
  
  ./ 同一级的文件
  ../上一级的文件
  
  #### rem匹配函数（凯源）
  ```function setFontSize() {
    let docEl = document.documentElement,
        resizeEvt = 'orientationchange' in window ? 'orientationchange' : 'resize',
        recalc = function () {
            let clientWidth = docEl.clientWidth;
            if (clientWidth >= 640) {
                clientWidth = 640
            }
            if (clientWidth === undefined) return;
            docEl.style.fontSize = 100 * (clientWidth / 640) + 'px';
        };
    if (document.addEventListener === undefined) return;
    window.addEventListener(resizeEvt, recalc, false);
    document.addEventListener('DOMContentLoaded', recalc, false);
    recalc();
}
```
