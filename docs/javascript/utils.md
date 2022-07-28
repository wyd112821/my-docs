# 常用工具函数封装

## 获取 URL 参数列表

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

## 键值对拼接成 URL 参数

```javascript
function params2Url(obj) {
    let params = [];
    for (let key in obj) {
        params.push(`${key}=${obj[key]}`);
    }
    return encodeURIComponent(params.join('&'));
}
```

## 修改 URL 中的参数

```javascript
function replaceParamVal(paramName, replaceWith) {
    const oUrl = location.href.toString();
    const re = eval('/(' + paramName + '=)([^&]*)/gi');
    location.href = oUrl.replace(re, paramName + '=' + replaceWith);
    return location.href;
}
```

## 删除 URL 中指定参数

```javascript
function funcUrlDel(name) {
    const baseUrl = location.origin + location.pathname + '?';
    const query = location.search.substr(1);
    if (query.indexOf(name) > -1) {
        const obj = {};
        const arr = query.split('&');
        for (let i = 0; i < arr.length; i++) {
            arr[i] = arr[i].split('=');
            obj[arr[i][0]] = arr[i][1];
        }
        delete obj[name];
        return (
            baseUrl +
            JSON.stringify(obj)
                .replace(/[\"\{\}]/g, '')
                .replace(/\:/g, '=')
                .replace(/\,/g, '&')
        );
    }
}
```

## 获取 URL 参数值

```javascript
function getUrlQuery(name) {
    // 获取参数
    var url = window.location.search;
    // 正则筛选地址栏
    var reg = new RegExp("(^|&)" + name + "=([^&]*)(&|$)");
    // 匹配目标参数
    var result = url.substr(1).match(reg);
    //返回参数值
    return result ? decodeURIComponent(result[2]) : null;
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

## 滚动到页面顶部

```javascript
function scrollToTop() {
    const height =
        document.documentElement.scrollTop || document.body.scrollTop;
    if (height > 0) {
        window.requestAnimationFrame(scrollToTop);
        window.scrollTo(0, height - height / 8);
    }
}
```

## 滚动到指定元素区域

```javascript
function smoothScroll(element) {
    document.querySelector(element).scrollIntoView({
        behavior: 'smooth',
    });
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

## 判断是否是苹果还是安卓移动设备

```javascript
function isAppleMobileDevice() {
    return /iphone|ipod|ipad|Macintosh/i.test(
        window.navigator.userAgent.toLowerCase()
    );
}
```

## 判断是否为数组

```javascript
function isArray(arr) {
  return Object.prototype.toString.call(arr) === '[object Array]';
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

## 数组拆分

```javascript
/**
 * @param {Array} arr 要拆分的数组
 * @param {Number} size 要拆分的数量
 */
function chunk(arr, size) {
    var result = [];
    for (var i = 0; i < Math.ceil(arr.length / size); i++) {
        var start = i * size;
        var end = start + size;
        result.push(arr.slice(start, end));
    }
    return result;
}
```

## 检测数组项是否存在

```javascript
/**
 * @param {String} item 检索的项
 * @param {Array} arr 所在数组
 */
function inArray(item, arr){
    for(var i in arr){
        if(arr[i] == item){
        return true;
        }
    }
    return false;
}
```

## 多层嵌套数据结构,根据最子级获取所有父级

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

## 生成指定范围随机数

```javascript
function randomNum(min, max) {
    return Math.floor(Math.random() * (max - min + 1)) + min;
}
```

## 数字千分位分隔

```javascript
function formatThousandth(n) {
    let num = n.toString();
    let len = num.length;
    if (len <= 3) {
        return num;
    } else {
        let remainder = len % 3;
        if (remainder > 0) {
            // 不是3的整数倍
            return (
                num.slice(0, remainder) +
                ',' +
                num.slice(remainder, len).match(/\d{3}/g).join(',')
            );
        } else {
            // 3的整数倍
            return num.slice(0, len).match(/\d{3}/g).join(',');
        }
    }
}
```

## 手机号中间四位变成\*

```javascript
function telFormat(tel) {
    tel = String(tel);
    return tel.substr(0, 3) + '****' + tel.substr(7);
}
```

## 格式化时间

```javascript
function dateFormater(formater, time) {
    let date = time ? new Date(time) : new Date(),
        Y = date.getFullYear() + '',
        M = date.getMonth() + 1,
        D = date.getDate(),
        H = date.getHours(),
        m = date.getMinutes(),
        s = date.getSeconds();
    return formater
        .replace(/YYYY|yyyy/g, Y)
        .replace(/YY|yy/g, Y.substr(2, 2))
        .replace(/MM/g, (M < 10 ? '0' : '') + M)
        .replace(/DD/g, (D < 10 ? '0' : '') + D)
        .replace(/HH|hh/g, (H < 10 ? '0' : '') + H)
        .replace(/mm/g, (m < 10 ? '0' : '') + m)
        .replace(/ss/g, (s < 10 ? '0' : '') + s);
}

// dateFormater('YYYY-MM-DD HH:mm:ss')
// dateFormater('YYYYMMDDHHmmss')
```

## 保留指定的小数位

```javascript
const toFixed = (n, fixed) => ~~(Math.pow(10, fixed) * n) / Math.pow(10, fixed);
// Examples
toFixed(25.198726354, 1);       // 25.1
toFixed(25.198726354, 2);       // 25.19
toFixed(25.198726354, 3);       // 25.198
toFixed(25.198726354, 4);       // 25.1987
toFixed(25.198726354, 5);       // 25.19872
toFixed(25.198726354, 6);       // 25.198726
```

## localStorage / sessionStorage 相关操作

```javascript
// 对本地存储的简单封装

//定义参数 类型 window.localStorage,window.sessionStorage,
const config = {
    type: 'localStorage', // 本地存储类型 localStorage/sessionStorage
    prefix: 'SDF_0.0.1', // 名称前缀 建议：项目名 + 项目版本
    expire: 1, //过期时间 单位：秒
}

// 名称前自动添加前缀
const autoAddPrefix = (key) => {
    const prefix = config.prefix ? config.prefix + '_' : '';
    return  prefix + key;
}

// 设置 setStorage
const setStorage = (key,value,expire=0) => {
    if (value === '' || value === null || value === undefined) {
        value = null;
    }

    if (isNaN(expire) || expire < 1) throw new Error("Expire must be a number");

    expire = (expire?expire:config.expire) * 60000;
    let data = {
        value: value, // 存储值
        time: Date.now(), //存值时间戳
        expire: expire // 过期时间
    }

    window[config.type].setItem(autoAddPrefix(key), JSON.stringify(data));
}

// 获取 getStorage
const getStorage = (key) => {
    let prefix_key = autoAddPrefix(key);
    // key 不存在判断
    if (!window[config.type].getItem(prefix_key) || JSON.stringify(window[config.type].getItem(prefix_key)) === 'null'){
        return null;
    }

    // 优化 持续使用中续期
    const storage = JSON.parse(window[config.type].getItem(prefix_key));
    let nowTime = Date.now();
    // 过期删除
    if (storage.expire && config.expire*6000 < (nowTime - storage.time)) {
        removeStorage(key);
        return null;
    } else {
        // 未过期期间被调用 则自动续期 进行保活
        setStorage(prefix_key,storage.value);
        return storage.value;
    }
}

// 获取全部 getAllStorage
const getAllStorage = () => {
    let len = window[config.type].length // 获取长度
    let arr = new Array() // 定义数据集
    for (let i = 0; i < len; i++) {
        // 获取key 索引从0开始
        let getKey = window[config.type].key(i)
        // 获取key对应的值
        let getVal = window[config.type].getItem(getKey)
        // 放进数组
        arr[i] = { 'key': getKey, 'val': getVal, }
    }
    return arr
}

// 删除 removeStorage
const removeStorage = (key) => {
    window[config.type].removeItem(autoAddPrefix(key));
}

// 清空 clearStorage
const clearStorage = () => {
    window[config.type].clear();
}

// 是否存在 hasStorage
const hasStorage = (key) => {
    key = autoAddPrefix(key);
    let arr = getStorageAll().filter((item)=>{
        return item.key === key;
    })
    return arr.length ? true : false;
}
```

## 判断是否为函数

```javascript
function isFunction(value) {
  return Object.prototype.toString.call(value) === '[object Function]'
}
```

## 删除左右两端空格

```javascript
function trim(str) {
  let reg = /^\s*|\s*$/g;　　
  return String(str).replace(reg, "");
}
```

## 禁止右键、选择、复制

```javascript
['contextmenu', 'selectstart', 'copy'].forEach(function(ev){
    document.addEventListener(ev, function(event){
        return event.returnValue = false
    })
});
```

## 函数只执行一次

```javascript
function once(fn) {
    let called = false
    return function () {
        if (!called) {
        called = true
        fn.apply(this, arguments)
        }
    }
}
```

### 类型判断

```javascript
/**
 * @param {*} target 目标
 * @returns {*} 返回类型 Array/Object/String/Number...
 */
function getType(target){
    return Object.prototype.toString.call(target).match(/\s+(\w+)/)[1];
}
```

### 对象数组深拷贝
```javascript
/**
 * @param obj 拷贝的目标
 * @returns {*}
 */
function deepCopy(obj){
    //判断是深拷贝对象还是数组
    let newObj = getType(obj) === "Array" ? [] : {};
    for (const i in obj) {
        if (typeof obj[i] === "object") {
        // 如果要拷贝的对象的属性依然是个复合类型，递归
        newObj[i] = deepCopy(obj[i]);
        } else {
        newObj[i] = obj[i];
        }
    }
    return newObj;
}
```