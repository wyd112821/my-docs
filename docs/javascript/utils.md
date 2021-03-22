# 常用工具函数封装

## 获取地址栏参数
```javascript
function getUrlParam(){
    var _arr = location.search.substr(1).split('&');
    var _obj = {};
    for (var i = 0; i < _arr.length; i++) {
        _obj[_arr[i].split('=')[0]] = _arr[i].split('=')[1]
    };
    return _obj;
}
```

## 动态追加Class
```javascript
function addClass (elem, className) {
  if (!className) return;

  const els = Array.isArray(elem) ? elem : [elem];

  els.forEach((el) => {
    if (el.classList) {
      el.classList.add(className.split(' '));
    } else {
      el.className += ` ${className}`;
    }
  });
}
```

## 动态删除Class
```javascript
function removeClass (elem, className) {
  if (!className) return;

  const els = Array.isArray(elem) ? elem : [elem];
  els.forEach((el) => {
    if (el.classList) {
      el.classList.remove(className.split(' '));
    } else {
      el.className = el.className.replace(new RegExp(`(^|\\b)${className.split(' ').join('|')}(\\b|$)`, 'gi'), ' ');
    }
  });
}
```

## js实现ajax方法
```javascript
function ajax () {
  var ajaxData = {
    type: arguments[0].type || "GET",
    url: arguments[0].url || "",
    async: arguments[0].async || "true",
    data: arguments[0].data || null,
    dataType: arguments[0].dataType || "text",
    contentType: arguments[0].contentType || "application/x-www-form-urlencoded",
    beforeSend: arguments[0].beforeSend || function () { },
    success: arguments[0].success || function () { },
    error: arguments[0].error || function () { }
  }
  ajaxData.beforeSend()
  var xhr = createxmlHttpRequest();
  xhr.responseType = ajaxData.dataType;
  xhr.open(ajaxData.type, ajaxData.url, ajaxData.async);
  xhr.setRequestHeader("Content-Type", ajaxData.contentType);
  xhr.send(convertData(ajaxData.data));
  xhr.onreadystatechange = function () {
    if (xhr.readyState == 4) {
      if (xhr.status == 200) {
        ajaxData.success(xhr.response)
      } else {
        ajaxData.error()
      }
    }
  }
}
// 创建XMLHttpRequest实例
function createxmlHttpRequest () {
  if (window.ActiveXObject) {
    return new ActiveXObject("Microsoft.XMLHTTP");
  } else if (window.XMLHttpRequest) {
    return new XMLHttpRequest();
  }
}
// 参数字符串化
function convertData (data) {
  if (typeof data === 'object') {
    var convertResult = "";
    for (var c in data) {
      convertResult += c + "=" + data[c] + "&";
    }
    convertResult = convertResult.substring(0, convertResult.length - 1)
    return convertResult;
  } else {
    return data;
  }
}
```

## 提示信息弹框
```javascript
function toast (msg, duration, callback) {
  duration = isNaN(duration) ? 3000 : duration;
  var father = document.createElement('div');
  var son = document.createElement('div');
  son.innerHTML = msg;
  son.className = "toast";
  father.className = "toast-wrapper";
  father.appendChild(son);
  document.body.appendChild(father);
  setTimeout(function () {
    var d = 0.5;
    father.style.opacity = '0';
    setTimeout(function () {
      document.body.removeChild(father);
      callback && callback();
    }, d * 1000);
  }, duration);
}
```

## 倒计时
```javascript
function countDown (obj, callback) {
  var count = 60;
  var resend = setInterval(function () {
    count--;
    if (count > 0) {
      obj.innerHTML = count + "秒后重发";
    } else {
      clearInterval(resend);
      obj.innerHTML = "获取验证码";
      callback();
    }
  }, 1000)
}
```

## 获取浏览器当前视口大小
```javascript
function getViewport(){
  if(document.compatMode == "BackCompat"){
    return{
      width: document.body.clientWidth,
      height: document.body.clientHeight
    }
  }else{
    return {
      width: document.documentElement.clientWidth,
      height: document.documentElement.clientHeight
    }
  }
}
```

## 判断是否移动设备访问
```javascript
function isMobileUserAgent() {  
  return (/iphone|ipod|android.*mobile|windows.*phone|blackberry.*mobile/i.test(window.navigator.userAgent.toLowerCase())); 
} 
```

## 从数组随机取不重复的值
```javascript
function randomArr(arr, num) {
  let newArr = [];
  for (let i = 0; i < num; i++) {
    let temp = Math.floor(Math.random() * arr.length);
    newArr.push(arr[temp]);
    arr.splice(temp, 1);
  }
  return newArr;
}
```
