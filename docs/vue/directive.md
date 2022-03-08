# 自定义指令

## v-copy
> 需求：实现一键复制文本内容，用于鼠标右键粘贴。

```javascript
// 思路：
// 1、动态创建 textarea 标签，并设置 readOnly 属性及移出可视区域
// 2、将要复制的值赋给 textarea 标签的 value 属性，并插入到 body
// 3、选中值 textarea 并复制
// 4、将 body 中插入的 textarea 移除
// 5、在第一次调用时绑定事件，在解绑时移除事件
const copy = {
  bind(el, { value }) {
    el.$value = value
    el.handler = () => {
      if (!el.$value) {
        // 值为空的时候，给出提示。可根据项目UI仔细设计
        console.log('无复制内容')
        return
      }
      // 动态创建 textarea 标签
      const textarea = document.createElement('textarea')
      // 将该 textarea 设为 readonly 防止 iOS 下自动唤起键盘，同时将 textarea 移出可视区域
      textarea.readOnly = 'readonly'
      textarea.style.position = 'absolute'
      textarea.style.left = '-9999px'
      // 将要 copy 的值赋给 textarea 标签的 value 属性
      textarea.value = el.$value
      // 将 textarea 插入到 body 中
      document.body.appendChild(textarea)
      // 选中值并复制
      textarea.select()
      const result = document.execCommand('Copy')
      if (result) {
        console.log('复制成功') // 可根据项目UI仔细设计
      }
      document.body.removeChild(textarea)
    }
    // 绑定点击事件，就是所谓的一键 copy 啦
    el.addEventListener('click', el.handler)
  },
  // 当传进来的值更新的时候触发
  componentUpdated(el, { value }) {
    el.$value = value
  },
  // 指令与元素解绑的时候，移除事件绑定
  unbind(el) {
    el.removeEventListener('click', el.handler)
  },
}

export default copy
```

```html
<!-- 使用：给 Dom 加上 v-copy 及复制的文本即可 -->
<template>
  <button v-copy="copyText">复制</button>
</template>

<script>
  export default {
    data() {
      return {
        copyText: 'a copy directives',
      }
    },
  }
</script>
```

## v-longpress

> 需求：实现长按，用户需要按下并按住按钮几秒钟，触发相应的事件

```javascript
// 思路：
// 1、创建一个计时器， 2 秒后执行函数
// 2、当用户按下按钮时触发 mousedown 事件，启动计时器；用户松开按钮时调用 mouseout 事件。
// 3、如果 mouseup 事件 2 秒内被触发，就清除计时器，当作一个普通的点击事件
// 4、如果计时器没有在 2 秒内清除，则判定为一次长按，可以执行关联的函数。
// 5、在移动端要考虑 touchstart，touchend 事件
const longpress = {
  bind: function (el, binding) {
    if (typeof binding.value !== 'function') {
      throw 'callback must be a function'
    }
    // 定义变量
    let pressTimer = null
    // 创建计时器（ 2秒后执行函数 ）
    let start = (e) => {
      if (e.type === 'click' && e.button !== 0) {
        return
      }
      if (pressTimer === null) {
        pressTimer = setTimeout(() => {
          handler()
        }, 2000)
      }
    }
    // 取消计时器
    let cancel = () => {
      if (pressTimer !== null) {
        clearTimeout(pressTimer)
        pressTimer = null
      }
    }
    // 运行函数
    const handler = (e) => {
      binding.value(e)
    }
    // 添加事件监听器
    el.addEventListener('mousedown', start)
    el.addEventListener('touchstart', start)
    // 取消计时器
    el.addEventListener('click', cancel)
    el.addEventListener('mouseout', cancel)
    el.addEventListener('touchend', cancel)
    el.addEventListener('touchcancel', cancel)
  },
  // 当传进来的值更新的时候触发
  componentUpdated (el, { value }) {
    el.$value = value
  },
  // 指令与元素解绑的时候，移除事件绑定
  unbind (el) {
    el.removeEventListener('click', el.handler)
  },
}

export default longpress
```

```html
<!-- 使用：给 Dom 加上 v-longpress 及回调函数即可 -->
<template>
  <button v-longpress="longpress">长按</button>
</template>

<script>
export default {
  methods: {
    longpress () {
      alert('长按指令生效')
    }
  }
}
</script>
```

## v-debounce

> 背景：在开发中，有些提交保存按钮有时候会在短时间内被点击多次，这样就会多次重复请求后端接口，造成数据的混乱，比如新增表单的提交按钮，多次点击就会新增多条重复的数据。

> 需求：防止按钮在短时间内被多次点击，使用防抖函数限制规定时间内只能点击一次。

```javascript
// 思路：
// 1、定义一个延迟执行的方法，如果在延迟时间内再调用该方法，则重新计算执行时间。
// 2、将事件绑定在 click 方法上。
const debounce = {
  inserted: function (el, binding) {
    let timer
    el.addEventListener('click', () => {
      if (timer) {
        clearTimeout(timer)
      }
      timer = setTimeout(() => {
        binding.value()
      }, 1000)
    })
  },
}

export default debounce
```

```html
<!-- 使用：给 Dom 加上 v-debounce 及回调函数即可 -->
<template>
  <button v-debounce="debounceClick">防抖</button>
</template>

<script>
export default {
  methods: {
    debounceClick () {
      console.log('只触发一次')
    }
  }
}
</script>
```

## v-lazyLoad

> 需求：实现一个图片懒加载指令，只加载浏览器可见区域的图片。

> IntersectionObserver 方案

```javascript
const lazyLoad = {
  bind: function(el) {
    el.onerror = () => {
      el.src = 'https://p9-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/532f0ad2675942efb12ab7f4efa7885a~tplv-k3u1fbpfcp-zoom-1.image'
    }

    if (!window.observer) {
      window.observer = new IntersectionObserver(entries => {
        entries.forEach(entry => {
          console.log('entry: ', entry);
          let lazyImage = entry.target
          if (entry.isIntersecting) {
            const src = lazyImage.getAttribute('data-src')
            lazyImage.src = src
            lazyImage.style.opacity = 1
            lazyImage.style.display = 'block'
            window.observer.unobserve(lazyImage)
          }
        })
      })
    }
  },
  inserted: el => {
    window.observer.observe(el)
  },
  componentUpdated: (el, binding) => {
    if (binding.value === binding.oldValue) {
      return false
    }
    window.observer.observe(el)
  },
  unbind: el => {
    window.observer.unobserve(el)
  }
}

export default lazyLoad
```

```html
<template>
  <img class="lazy-img" :data-src="src" v-lazyLoad="src" />
</template>

<script>
export default {
  data() {
    return {
      src: 'logo.png'
    }
  }
}
</script>
```

> onscroll 方案

```javascript
const lazyLoad = {
  bind: function(el) {
    // set default src when load onerror
    el.onerror = () => {
      el.src =
        'https://p9-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/532f0ad2675942efb12ab7f4efa7885a~tplv-k3u1fbpfcp-zoom-1.image'
    }

    if (!window.lazyMap) {
      // 创建 map 来缓存
      window.lazyMap = new Map()

      // 监听滚动事件，添加方都处理
      window.onscroll = throttle(() => {
        window.lazyMap.forEach((lazyImg, key) => {
          if (isElementInViewport(lazyImg.el)) {
            lazyImg.el.src = lazyImg.value.src
            lazyImg.value.callback(lazyImg.el)
            window.lazyMap.delete(key)
          }
        })
      }, 200)
    }
  },
  inserted: (el, binding) => {
    lazyImgAction(el, binding)
  },
  componentUpdated: (el, binding) => {
    // if the src not change
    if (binding.value.src === binding.oldValue.src) {
      return false
    }
    lazyImgAction(el, binding)
  },
  unbind: (el, binding) => {
    const key = binding.value.src
    if (window.lazyMap && window.lazyMap.has(key)) {
      window.lazyMap.delete(key)
    }
  }
}

export default lazyLoad
```

```html
<template>
  <img class="lazy-img" v-lazyLoad="lazyOptions" />
</template>

<script>
export default {
  data() {
    return {
      src: 'logo.png'
    }
  },
  computed: {
    lazyOptions() {
      return {
        src: this.src,
        callback: this.onCallback
      }
    }
  },
  methods: {
    onCallback(el) {
      el.style.opacity = 1
      el.style.display = 'block'
    }
  }
}
</script>
```

## v-permission

> 需求：自定义一个权限指令，对需要权限判断的 Dom 进行显示隐藏。

```javascript
// 思路：
// 1、自定义一个权限数组
// 2、判断用户的权限是否在这个数组内，如果是则显示，否则则移除 Dom
function checkArray (key) {
  let arr = ['1', '2', '3', '4']
  let index = arr.indexOf(key)
  if (index > -1) {
    return true // 有权限
  } else {
    return false // 无权限
  }
}

const permission = {
  inserted: function (el, binding) {
    let permission = binding.value // 获取到 v-permission的值
    if (permission) {
      let hasPermission = checkArray(permission)
      if (!hasPermission) {
        // 没有权限 移除Dom元素
        el.parentNode && el.parentNode.removeChild(el)
      }
    }
  },
}

export default permission
```

```html
<template>
  <div class="btns">
    <!-- 显示 -->
    <button v-permission="permission1">权限按钮1</button>
    <!-- 不显示 -->
    <button v-permission="permission2">权限按钮2</button>
  </div>
</template>

<script>
export default {
  data() {
    return {
      permission1: '0',
      permission2: '1'
    }
  }
}
</script>
```

## v-draggable

> 需求：实现一个拖拽指令，可在页面可视区域任意拖拽元素。

```javascript
// 思路：
// 1、设置需要拖拽的元素为相对定位，其父元素为绝对定位。
// 2、鼠标按下(onmousedown)时记录目标元素当前的 left 和 top 值。
// 3、鼠标移动(onmousemove)时计算每次移动的横向距离和纵向距离的变化值，并改变元素的 left 和 top 值
// 4、鼠标松开(onmouseup)时完成一次拖拽
const draggable = {
  inserted: function (el) {
    el.style.cursor = 'move'
    el.onmousedown = function (e) {
      let disx = e.pageX - el.offsetLeft
      let disy = e.pageY - el.offsetTop
      document.onmousemove = function (e) {
        let x = e.pageX - disx
        let y = e.pageY - disy
        let maxX = document.body.clientWidth - parseInt(window.getComputedStyle(el).width)
        let maxY = document.body.clientHeight - parseInt(window.getComputedStyle(el).height)
        if (x < 0) {
          x = 0
        } else if (x > maxX) {
          x = maxX
        }

        if (y < 0) {
          y = 0
        } else if (y > maxY) {
          y = maxY
        }

        el.style.left = x + 'px'
        el.style.top = y + 'px'
      }
      document.onmouseup = function () {
        document.onmousemove = document.onmouseup = null
      }
    }
  },
}

export default draggable
```

```html
<template>
  <div class="wrapper">
    <div class="el-dialog" v-draggable></div>
  </div>
</template>

<style scoped>
.wrapper{
  position: relative;
  width: 100%;
  height: calc(100vh);
}
.el-dialog {
  position: absolute;
  left: 0;
  top: 0;
  width: 100px;
  height: 100px;
  background-color: #f8f8f8;
}
</style>
```

## v-waterMarker
> 需求：给整个页面添加背景水印

```javascript
// 思路：
// 使用 canvas 特性生成 base64 格式的图片文件，设置其字体大小，颜色等。
// 将其设置为背景图片，从而实现页面或组件水印效果

function addWaterMarker (str, parentNode, font, textColor) {
  // 水印文字，父元素，字体，文字颜色
  var canvas = document.createElement('canvas');
  parentNode.appendChild(canvas);
  canvas.width = 200;
  canvas.height = 150;
  canvas.style.display = 'none';
  var context = canvas.getContext('2d');
  context.rotate(-20 * Math.PI / 180);
  context.font = font || '16px Microsoft JhengHei';
  context.fillStyle = textColor || 'rgba(180, 180, 180, 0.3)';
  context.textAlign = 'left';
  context.textBaseline = 'middle';
  context.fillText(str, canvas.width / 10, canvas.height / 2);
  parentNode.style.backgroundImage = 'url(' + canvas.toDataURL('image/png') + ')';
}

const waterMarker = {
  bind: function (el, binding) {
    addWaterMarker(binding.value.text, el, binding.value.font, binding.value.textColor)
  },
}

export default waterMarker
```

```html
<template>
  <div v-waterMarker="{text:'版权所有',textColor:'rgba(180, 180, 180, 0.4)'}"></div>
</template>
```

## v-tooltip
> 需求：给绑定的节点添加文字提示说明框

```javascript
// 思路：
// 结合antd框架中的tooltip组件
// 通过传给指令参数和绑定值来为元素添加说明

function structureIcon (content, attrs) {
  //拼接绑定属性
  let attrStr = '';
  for (let key in attrs) {
    attrStr += `${key}=${attrs[key]}`;
  }
  const a = `<a-tooltip title='${content}' ${attrStr}></a-tooltip>`;
  // 创建构造器
  const tooltip = Vue.extend({
    template: a
  })
  // 创建一个 tooltip 实例并返回 dom 节点
  const component = new tooltip().$mount()
  return component.$el
}

const tooltip = {
  bind: function (el, binding) {
    if (el.hasIcon) return;
    const iconElement = structureIcon(binding.arg, binding.value);
    iconElement.innerHTML = el.innerHTML;
    el.innerHTML = '';
    el.appendChild(iconElement);
    el.hasIcon = true;
  }
}

export default tooltip
```

```html
<template>
  <div v-tooltip:提示内容为XXX='tootipParams'> 提示2 </div>
</template>
```