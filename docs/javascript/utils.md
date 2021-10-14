# 常用工具函数封装

## 获取地址栏参数

```javascript
function getUrlParam() {
    var _arr = location.search.substr(1).split('&');
    var _obj = {};
    for (var i = 0; i < _arr.length; i++) {
        _obj[_arr[i].split('=')[0]] = _arr[i].split('=')[1];
    }
    return _obj;
}
```

## 动态追加 Class

```javascript
function addClass(elem, className) {
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

## 动态删除 Class

```javascript
function removeClass(elem, className) {
    if (!className) return;

    const els = Array.isArray(elem) ? elem : [elem];
    els.forEach((el) => {
        if (el.classList) {
            el.classList.remove(className.split(' '));
        } else {
            el.className = el.className.replace(
                new RegExp(
                    `(^|\\b)${className.split(' ').join('|')}(\\b|$)`,
                    'gi'
                ),
                ' '
            );
        }
    });
}
```

## js 实现 ajax 方法

```javascript
function ajax() {
    var ajaxData = {
        type: arguments[0].type || 'GET',
        url: arguments[0].url || '',
        async: arguments[0].async || 'true',
        data: arguments[0].data || null,
        dataType: arguments[0].dataType || 'text',
        contentType:
            arguments[0].contentType || 'application/x-www-form-urlencoded',
        beforeSend: arguments[0].beforeSend || function () {},
        success: arguments[0].success || function () {},
        error: arguments[0].error || function () {},
    };
    ajaxData.beforeSend();
    var xhr = createxmlHttpRequest();
    xhr.responseType = ajaxData.dataType;
    xhr.open(ajaxData.type, ajaxData.url, ajaxData.async);
    xhr.setRequestHeader('Content-Type', ajaxData.contentType);
    xhr.send(convertData(ajaxData.data));
    xhr.onreadystatechange = function () {
        if (xhr.readyState == 4) {
            if (xhr.status == 200) {
                ajaxData.success(xhr.response);
            } else {
                ajaxData.error();
            }
        }
    };
}
// 创建XMLHttpRequest实例
function createxmlHttpRequest() {
    if (window.ActiveXObject) {
        return new ActiveXObject('Microsoft.XMLHTTP');
    } else if (window.XMLHttpRequest) {
        return new XMLHttpRequest();
    }
}
// 参数字符串化
function convertData(data) {
    if (typeof data === 'object') {
        var convertResult = '';
        for (var c in data) {
            convertResult += c + '=' + data[c] + '&';
        }
        convertResult = convertResult.substring(0, convertResult.length - 1);
        return convertResult;
    } else {
        return data;
    }
}
```

## 提示信息弹框

```javascript
function toast(msg, duration, callback) {
    duration = isNaN(duration) ? 3000 : duration;
    var father = document.createElement('div');
    var son = document.createElement('div');
    son.innerHTML = msg;
    son.className = 'toast';
    father.className = 'toast-wrapper';
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
function countDown(obj, callback) {
    var count = 60;
    var resend = setInterval(function () {
        count--;
        if (count > 0) {
            obj.innerHTML = count + '秒后重发';
        } else {
            clearInterval(resend);
            obj.innerHTML = '获取验证码';
            callback();
        }
    }, 1000);
}
```

## 获取浏览器当前视口大小

```javascript
function getViewport() {
    if (document.compatMode == 'BackCompat') {
        return {
            width: document.body.clientWidth,
            height: document.body.clientHeight,
        };
    } else {
        return {
            width: document.documentElement.clientWidth,
            height: document.documentElement.clientHeight,
        };
    }
}
```

## 判断是否移动设备访问

```javascript
function isMobileUserAgent() {
    return /iphone|ipod|android.*mobile|windows.*phone|blackberry.*mobile/i.test(
        window.navigator.userAgent.toLowerCase()
    );
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

## 数组去重

```javascript
// 双层for循环去重
function unique(arr) {
    for (var i = 0; i < arr.length; i++) {
        for (var j = i + 1; j < arr.length; j++) {
            if (arr[i] == arr[j]) {
                arr.splice(j, 1);
                j--;
            }
        }
    }
    return arr;
}

// 利用indexOf去重
function unique(arr) {
    if (!Array.isArray(arr)) {
        console.log('type error!');
        return;
    }
    var array = [];
    for (var i = 0; i < arr.length; i++) {
        if (array.indexOf(arr[i]) === -1) {
            array.push(arr[i]);
        }
    }
    return array;
}
```

## 数组扁平化

```javascript
// 递归
function flatten(arr) {
    var result = [];
    for (var i = 0, len = arr.length; i < len; i++) {
        if (Array.isArray(arr[i])) {
            result = result.concat(flatten(arr[i]));
        } else {
            result.push(arr[i]);
        }
    }
    return result;
}

// reduce
function flatten(arr) {
    return arr.reduce(function (prev, next) {
        return prev.concat(Array.isArray(next) ? flatten(next) : next);
    }, []);
}
```

## 多层嵌套数据结构,根据最子级获取所以父级

```javascript
function childFindParents(target, val) {
    for (var i = 0; i < target.length; i++) {
        if (target[i] && target[i].value == val) {
            return [];
        }
        if (target[i].children && target[i].children.length) {
            var parents = childFindParents(target[i].children, val);
            if (parents) {
                return parents.concat(target[i].value);
            }
        }
    }
}
```

## 函数防抖

> 触发高频事件后 n 秒内函数只会执行一次，如果 n 秒内高频事件再次被触发，则重新计算时间

```javascript
function debounce(fn) {
    let timeout = null; // 创建一个标记用来存放定时器的返回值
    return function () {
        clearTimeout(timeout); // 每当用户输入的时候把前一个 setTimeout clear 掉
        timeout = setTimeout(() => {
            // 然后又创建一个新的 setTimeout, 这样就能保证输入字符后的 interval 间隔内如果还有字符输入的话，就不会执行 fn 函数
            fn.apply(this, arguments);
        }, 500);
    };
}
```

## 函数节流

> 高频事件触发，但在 n 秒内只会执行一次，所以节流会稀释函数的执行频率

```javascript
function throttle(fn) {
    let canRun = true; // 通过闭包保存一个标记
    return function () {
        if (!canRun) return; // 在函数开头判断标记是否为true，不为true则return
        canRun = false; // 立即设置为false
        setTimeout(() => {
            // 将外部传入的函数的执行放在setTimeout中
            fn.apply(this, arguments);
            // 最后在setTimeout执行完毕后再把标记设置为true(关键)表示可以执行下一次循环了。当定时器没有执行的时候标记永远是false，在开头被return掉
            canRun = true;
        }, 500);
    };
}
```

## cookie 相关操作

```javascript
function set_c(C, D, h) {
    exp = new Date();
    exp.setTime(exp.getTime() + 3600 * 1000 * h);
    document.cookie =
        C + '=' + escape(D) + '; expires=' + exp.toGMTString() + '; path=/';
}

function get_c(C) {
    var D;
    D = C + '=';
    offset = document.cookie.indexOf(D);
    if (offset != -1) {
        offset += D.length;
        end = document.cookie.indexOf(';', offset);
        if (end == -1) {
            end = document.cookie.length;
        }
        return unescape(document.cookie.substring(offset, end));
    } else {
        return '';
    }
}

function del_c(B) {
    exp = new Date();
    exp.setTime(exp.getTime() - 10000000);
    document.cookie =
        B + '=' + null + '; expires=' + exp.toGMTString() + '; path=/';
}
```
