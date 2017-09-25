---
title: 边写边学vue之二———数据传递
date: 2017-09-01 21:35:33
tags:
---

### 1. fetch

- 原生不支持跨域，需要设置后端代理
- 虽然设置了`{mode:'no-cors'}`不会报错，但结果会返回`type:"opaque",ok: 'false'`表明没有权限访问
- 写法
```
<!-- 链接页面 -->
fetch('url')
<!-- 处理数据并返回，千万不要忘记return  -->
.then(res=>return res.text())
<!-- 处理data -->
.then(data=>console.log(data))
```

### 2. Jsonp

1. 原理： script标签可以跨域访问，并且把返回的内容作为callback函数参数,可以对其进行处理
2. 写法
```
<!-- 这里的data就是豆瓣API接口返回的数据 -->
var fn = function(data){
    console.log(data)
}
let search = document.createElement('script');//创建script标签
search.src="https://api.douban.com/v2/book/search?q=虚构类&callback=fn";//src引入链接，并通过fn对数据做处理
document.body.appendChild(search)//添加到页面
```

### 3. mutation 和 action
- 都是对state数据的处理
- mutation 是同步的，用this.$store.commit提交
- action 是异步的，用this.$store.dispatch提交
  - dispatch提交了一个action,action处理完成之后再commit给mutation修改state
- 所以，最终的state修改都是mutation处理的

### 4. VueX中actions里的commit函数的参数解构形式

1. `commit()`可以传入mutations里的函数名的字符串，来确认提交一个mutation
`commit('fn')`

2. 也可以传入一个对象，该对象t的type属性就是提交的mutation函数，其他属性可以自行设置,该对象作为mutation函数的第二个参数，可以在mutation里处理
```
commit({  
    type: fn,  
    key: value,  
})
```

3. actions提交mutation
```
//context是一个与store实例具有相同方法和属性的对象，通过调用context下的commit方法提交mutation
actions: {
    fn(context){
        context.commit('mutationFn')
    }
}
// 根据参数解构（传参对象的值可以直接使用）,这里函数内部只使用了context的commit属性，所以可以简写为
actions:{
    fn({commit:commit}){
        commit('mutationFn')
    }
}
//进一步简写为
actions:{
    fn({commit}){
        commit('mutationFn')
    }
}
```

### 5. asnyc 和 await

- 解决异步问题
- 写法：
```
var fn = asnyc function(){
   await fn1
   await fn2
}
fn()
```

### 6. Vue跨域

- webpack配置proxyTable的参数(在config目录下的index.js中)
- 配置方法
```
proxyTable: {
    '/api': {
        target: 'http://api.douban.com/v2',
        changeOrigin: true,
        pathRewrite: {
            '^/api': ''
        }
    }
```
- 调用方法
    `/api/xxxxxx`

### 7. mapState

- state 的辅助函数，生成计算属性，参数是一个对象，返回一个对象
- 在通过computed传给计算属性
- 通过…扩展运算符简写
```
//单组件引入mapstate
import {mapState} from "vuex"
computed: {
    ...mapState({
         attr: state=>state.attr 
        })
}
```

### 8. 箭头函数

```
a=>a
//等同于
function (a){ return a }

ar f = () => 5;
// 等同于
var f = function () { return 5 };

var sum = (num1, num2) => num1 + num2;
// 等同于
var sum = function(num1, num2) {
  return num1 + num2;
};
```