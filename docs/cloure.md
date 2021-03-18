# 介绍闭包
## 什么是闭包
哈哈哈哈

## 闭包的作用是什么
12312313

## 测试
测试00012132

### 测试001
![docsify](https://docsify.js.org/_media/icon.svg)

```html
<p>This is a paragraph</p>
<a href="//docsify.js.org/">Docsify</a>
```

```css
//pc端

@charset "utf-8";
/*----------- 重置样式 ----------------*/
html{-webkit-text-size-adjust:none;}
*html{background-image:url(about:blank); background-attachment:fixed;}/*解决IE6下滚动抖动的问题*/
*{margin: 0; padding: 0; list-style: none;}
q:before,q:after{content:'';}
abbr,acronym{border:0;}
body{padding:0; margin:0; font: 14px/1.5 "Microsoft YaHei",微软雅黑,'黑体','宋体',tahoma,Verdana,arial,sans-serif; color:#333; background: url(../images/public/body_bg.png) #f5f5f5 center top no-repeat; min-width: 1200px;}
input,textarea,select,button,label{vertical-align:middle; font-family:"Microsoft YaHei",微软雅黑;}
textarea{resize:none;}
ul,ol,li,dl,dt,dd,h1,h2,h3,h4,h5,h6,p,img,a,form,input,label,select{margin:0; padding:0; list-style:none;}
a{color:#333; text-decoration:none; outline:none; -webkit-transition: all 0.4s; transition: all 0.4s;}
a:hover{color:#045da6; text-decoration:none; -webkit-transition: all 0.4s; transition: all 0.4s;}
a img{border:none;}
.fl{float:left;}
.fr{float:right;}
.clear{margin:0; overflow:hidden; visibility:hidden; font-size: 0; content: "."; clear: both; height: 0; padding:0;}
.clearfix:after{visibility:hidden; display: block; font-size: 0; content:" "; clear:both; height:0;}
* html .clearfix{zoom: 1;} /* IE6 */
*:first-child+html .clearfix{zoom: 1;} /* IE7 */
h1,h2,h3{font-weight: normal;}
/*--- 自定义全局function ---*/
.clearfix {
    *zoom: 1;
}
.clearfix:before,
.clearfix:after {
    display: table;
    line-height: 0;
    content: "";
}
.clearfix:after {
    clear: both;
}
.hide {
    display: none;
}
.show {
    display: block;
}

```

```bash
echo "hello"
```

```javascript
//获取地址栏参数
function getUrlParam(){
    var _arr = location.search.substr(1).split('&');
    var _obj = {};
    for (var i = 0; i < _arr.length; i++) {
        _obj[_arr[i].split('=')[0]] = _arr[i].split('=')[1]
    };
    return _obj;
}
console.log(getUrlParam());
```


## 修改

## diff
