---
title: 真正能让input标签一直保持焦点的办法！
date: 2022-12-16 20:27:21
tags:
  - 前端
  - javascript
  - 教程
draft: true
---

我在做一个类似命令行的页面，输入用**input**标签，想保持焦点，不会。（刚入门，学艺不精啊）

网络上找了半天，终于拼出了正确内容。记录如下

首先，在**head**标签里添加一个**script**标签，写入如下代码
```Javascript
function refocus(e){
    var that= this;
    setTimeout(function (){
        console.log(that);
        document.getElementById("input标签的ID").focus();
    },100);
    
}
```

在你想保持焦点的input标签上加上: **onblur="refocus(this);"**
如这样:
```HTML
<input id="input标签的ID" type="text" onblur="refocus(this);" />
```
记得把上面的ID替换成有意义的ID

