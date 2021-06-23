# 常用css代码块

## 常用css reset样式
```css
//pc端
@charset "utf-8";
/*----------- 重置样式 ----------------*/
html{-webkit-text-size-adjust:none;}
*html{background-image:url(about:blank); background-attachment:fixed;}/*解决IE6下滚动抖动的问题*/
*{margin: 0; padding: 0; list-style: none;}
q:before,q:after{content:'';}
abbr,acronym{border:0;}
body{padding:0; margin:0; font: 14px/1.5 "Microsoft YaHei",微软雅黑,'黑体','宋体',tahoma,Verdana,arial,sans-serif; color:#333; background-color: #fff; min-width: 1200px;}
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
/* 去掉number输入框右边点击上下的小三角 */
input::-webkit-inner-spin-button{-webkit-appearance: none;}
input::-webkit-outer-spin-button{-webkit-appearance: none;}
/*placeholder 文字颜色设置*/
input::-moz-placeholder,textarea::-moz-placeholder{color: #828897;opacity: 1; font-weight: normal;}
input:-ms-input-placeholder,textarea:-ms-input-placeholder{color: #828897;opacity: 1; font-weight: normal;}
input::-webkit-input-placeholder,textarea::-webkit-input-placeholder{color: #828897;opacity: 1; font-weight: normal;}
/* Chrome浏览器会在输入控制聚集的时候添加一个蓝色的outline*/
input:focus, textarea:focus, select:focus{outline: none!important;}
/* 消除input元素 type="number" 时默认的 加减按钮*/
input[type=number]::-webkit-inner-spin-button,
input[type=number]::-webkit-outer-spin-button{-webkit-appearance: none; margin: 0;}
/* 消除input元素 type="number" 时默认的 加减按钮---moz版*/
input[type=number]{-moz-appearance:textfield;}
/* 去掉select的默认样式 */
select{-webkit-appearance: none;}
/* 重置浏览器滚动条的默认样式 */
::-webkit-scrollbar{width: 10px; height: 10px; overflow: visible}
::-webkit-scrollbar-thumb{border: solid transparent; border-width: 1px 0; background-clip: padding-box; background: #c6c8cc}
::-webkit-scrollbar-thumb:hover{background-color: #9198a6}
::-webkit-scrollbar-button{width: 0; height: 0}
::-webkit-scrollbar-button:hover{border-color: #5f6366}
::-webkit-scrollbar-button:vertical:end:decrement,::-webkit-scrollbar-button:vertical:start:increment{display: none}

//移动端
@charset "utf-8";
html,body,h1,h2,h3,h4,h5,h6,div,dl,dt,dd,ul,ol,li,p,blockquote,pre,hr,figure,table,caption,th,td,form,fieldset,legend,input,button,textarea,menu{margin:0;padding:0;}
header,footer,section,article,aside,nav,hgroup,address,figure,figcaption,menu,details{display:block;}
table{border-collapse:collapse;border-spacing:0;}
caption,th{font-weight:normal;}
html,body,fieldset,iframe,abbr{border:0;}
i,cite,em,var,address,dfn{font-style:normal;}
[hidefocus],summary{outline:0;}
ul , ol , ul li , li , ol li{list-style:none;}
h1,h2,h3,h4,h5,h6,small{font-size:100%;}
sup,sub{font-size:83%;}
pre,code,kbd,samp{font-family:inherit;}
q:before,q:after{content:none;}
textarea{overflow:auto;resize:none;}
label,summary{cursor:default;}
a,button{cursor:pointer;}
h1,h2,h3,h4,h5,h6,em,strong,b{font-weight:bold;}
ins,u,s,a,a:hover{text-decoration:none;}
body,textarea,input,button,select,keygen,legend{font-family: "Helvetica Neue", Helvetica, Arial, "PingFang SC", "Hiragino Sans GB", "Heiti SC", "Microsoft YaHei", "WenQuanYi Micro Hei", sans-serif; color:#666;outline:0;}
a{color:#333; border: none; text-decoration: none;outline:none; /*移除虚线框  IE8,FF有用*/ hide-focus: expression(this.hideFocus=true); /*IE6、IE7*/}
a:focus{outline: 0;-moz-outline-style: none;}
a:hover{color: #0065bb;}
img{overflow: hidden;border: 0 none;vertical-align: middle;-ms-interpolation-mode: bicubic;}
button,input,select,textarea{font-size:100%;font-family:tahoma;margin: 0;outline: 0 none;vertical-align: baseline;_overflow:visible;*vertical-align: middle;*overflow:visible;}
label,
select,
button,
input[type="button"],
input[type="reset"],
input[type="submit"],
input[type="radio"],
input[type="checkbox"] {cursor: pointer;}
html { font-size : 20px;}
.clearfix { *zoom: 1;}
.clearfix:before,
.clearfix:after {display: table; line-height: 0; content: "";}
.clearfix:after {clear: both;}
.hide {display: none;}
.show {display: block;}
```

## 网页元素CSS居中实现方法
> 水平居中：多个块状元素解决方案 (使用flexbox布局实现)
>> 使用flexbox布局，只需要把待处理的块状元素的父元素添加属性display:flex及justify-content:center即可:
```css
.parent {
  display:flex;  
  justify-content:center; 
}
```

> 垂直居中：多行的行内元素解决方案
>> 组合使用display:table-cell和vertical-align:middle属性来定义需要居中的元素的父容器元素生成效果，如下：
```css
.parent {
  background: #222;
  width: 300px;
  height: 300px;
  /* 以下属性垂直居中 */
  display: table-cell;
  vertical-align:middle;
}
```

> 垂直居中：未知高度的块状元素解决方案
```css
div{
  top: 50%;
  position: absolute;
  transform: translateY(-50%);  /* 使用css3的transform来实现 */
}
```

> 水平垂直居中：已知高度和宽度的元素解决方案
```css
div{
  position: absolute;
  margin:auto;
  left:0;
  top:0;
  right:0;
  bottom:0;
}
```

> 水平垂直居中：未知高度和宽度元素解决方案
```css
div{
  position: absolute;
  top: 50%;
  left: 50%;
  transform: translate(-50%, -50%);  /* 使用css3的transform来实现 */
}
```

> 水平垂直居中：使用flex布局实现
```css
.parent{
  display: flex;
  justify-content:center;
  align-items: center;
  /* 注意这里需要设置高度来查看垂直居中效果 */
  background: #AAA;
  height: 300px;
}
```

## 单行文本超过长度时显示省略号
> 需要给定宽度，行内元素也要先转为块或行内块才能生效
```css
div{
  width:400px;
  height:30px;
  overflow: hidden;
  text-overflow:ellipsis;
  white-space: nowrap;
}
```

## 多行文本超过长度时显示省略号
```css
div{
  overflow: hidden;
  text-overflow: ellipsis;
  display: -webkit-box;
  -webkit-line-clamp: 1; /* 可以设置多行 */
  -webkit-box-orient: vertical;
  white-space: normal !important;
  word-wrap: break-word;
}
```

## 利用margin和padding实现左右等高布局
```css
.box{ 
  overflow:hidden; 
  resize:vertical;
}
.child-left, .child-right{
  margin-bottom:-600px;
  padding-bottom:600px;
}
.child-left{
  float:left;
  background:orange;
}
.child-right{
  float:left;
  background:red;
}
```