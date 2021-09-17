---
title: fetch详解
date: 2021-06-12 19:47:48
tags: js
categoriese: 笔记
---

今天详细学一下fetch的用法
<!--more-->

# 基本用法

## 分派请求和读取响应 
对于fetch的用法在上一篇Blog中有了基本的了解，现在我们在深入的看一下fetch的用法。
都知道，fetch是异步的，是关注分离的，fetch有一个必须的参数，就是URL，利用fetch获取URL资源后会返回一个pending状态的Promise。

```js
let p = fetch(`url`);
console.log(p); // Promise(pending)
```

请求URL完成且资源可用后，Promise会生成一个Response对象。这个对象是API的封装，可以通过使用这个对象的属性和方法取得相应资源。
用text方法可以读取response对象的纯文本格式内容。

`response.text()`

## 处理状态码和请求失败
我们首先要知道一点，就是fetch在接收到一个代表错误的 HTTP 状态码时，从 fetch() 返回的 Promise 不会被标记为 reject， 即使响应的 HTTP 状态码是 404 或 500。相反，它会将 Promise 状态标记为 resolve （但是会将 resolve 的返回值的 ok 属性设置为 false ），仅当网络故障时或请求被阻止时，才会标记为 reject。

也就是说，只有你断网了或请求错了，fetch啥东西也得不到，才会返回一个reject状态的Promise。从我自己的理解来看，fetch的意思就是取嘛，404，500 都是你访问到了，虽然取不到ok状态的response，但你好歹取到了404 500状态码和一个不知道什么鬼且ok是false的response，是吧，只要能取到东西fetch返回到Promise可以是成功的，就不会被报错。

所以想判断fetch请求后的具体响应状态，就要用下面这些方法了。

```js
response.status() //返回http状态码
response.statysText() // 返回状态文本 比如 200 - OK 404 - Not Found 500 - Internal Server Error
response .ok() // 询问请求是否成功，true 对应 200 - 299 状态码，其他都是false
responese.url() // 返回请求的 URL。如果 URL 存在跳转，该属性返回的是最终 URL。
```

## 其余参数

如果只传一个URL参数，则fetch会默认发送GET请求，要想进一步配置请求方式，则要传入第二个参数————一个init对象，init对象要按照下表的key/value填充
(具体的就去MDN上查，这里只写一些常用的)

```js
{
    method: //具体的请求方法 ，“GET”，“POST”等
    headers: //一个用来定制HTTP标头的对象
    body: //POST请求的请求体
}
```


# 常见的Fetch请求模式
这里是一些关于init对象的配置使用

## 发送JSON数据

```js
let postData = JSON.stringify({
    name: 'xyy'
});

fetch (`url`),{
    method: 'POST',
    body: postDate,
    headers: {
        'Content-Type': 'application/json'
    }
}
```
## 在请求体中发送参数

```js
let postData = 'foo=bar&lorem=ipsum';

fetch (`url`),{
    method: 'POST',
    body: postDate,
    headers: {
    "Content-type": "application/x-www-form-urlencoded; charset=UTF-8",
  },

}
```

## 提交表单

```js
const form = document.querySelector('form');

const response = await fetch('/users', {
  method: 'POST',
  body: new FormData(form)
})
```

## 上传文件
因为请求体支持FormData实现，所以fetch()就可以序列化并发送文件字段中的文件:

```js
let imageFormData = new FormData();
let imageInput = document.querySelector("input[type='file']");

imageFormData.append('image',imageInput.file[0]);

// 实现多个文件:
// let imageInput = document.querySelector("input[type='file'][multiple]");
// for(let i = 0;i < imageInput.fileslength; ++i)
// imageFormData.append('image',imageInput.file[i]);

fetch(`url`,{
    method:'POST',
    body: imageFormData
});
```

具体更高深的就自己去看MDN吧。

# 其他对象

## Headers对象
Response对象里面有一个headers属性，指向Header对象，Headers对象就是所有外发请求和入站响应头部的容器，每个外发的Request实例都包涵一个空的Headers实例，可以通过Request.headers访问。

1. headers对象形式：
headers对象与Map相似，因为Http头部本质上就是序列化后的键值对，所以Headers对象有get,set,has,delete等实例方法，也有相同的keys(),values()和entries()迭代器接口,对于这些方法的运用还是去看看Map吧，这里就不详细赘述了。

2. headers独有的特性
headers初始化的时候可以直接用普通obj形式对象(也就是{}包裹的键/值对对象)，Headers对象可以由append()方法添加多个值，调用不存在的值等同于调用set(),若是用append()多次添加一个键名，则该键名对应的值以逗号为分隔符显示。

3. 头部护卫
就是限制Headers对象不被修改的一些知识，自行了解。

# Request对象
顾名思义，Request对象是获取资源请求的接口。这个接口暴露了请求的相关信息，也暴露了使用请求题的不同方式。

## 创建Request对象
通过构造函数初始化Request对象，为此传入一个input参数，一般是URL;

Request对象也接收第二个参数，一个init对象，和fetch中的第二个参数一样，不加配置的话就传入默认值。

```js
let r = new Request(`url`，{/*一些key/value*/});
```

## 克隆Request对象
除了构造函数创建Request对象外，还可以用clone()方法，

有两种方法可以克隆一个Request对象

```js
// 一.把Request实例作为input参数传给Request构造函数
let r1 = new Request(`url`);
let r2 = new Request(r1); // 如果传入init对象，则会覆盖。
console.log(r1.bodyused); // true
//不能克隆一摸一样的值，r1会被标记为已使用，bodyused的值为true
//还有一些关于referrer和mode属性的改变，具体属性可以自己去了解一下。

// 二.Request.clone()
let r1 = new Request(`url`);
let r2 = r1.clone() //这个方法会让创建两个一摸一样的副本,bodyused的值为false

console.log(r1===r2) // true

```

如果请求对象的bodyused值为true，那么就不能对该Request对象创建副本。

## 在fetch()中使用Request对象
fetch()和Request构造函数拥有相同的函数签名不是巧合，在调用fetch()的时候，可以传入已经构造好的Request实例而不是URL，和Request构造函数一样，传给fetch()的init对象会覆盖默认值。

关键点在于，通过fetch()使用Request实例后会把其bodyused属性设置为true，要想对同一个Request实例多次调用fetch(),必须在第一次发送fetch()请求前调用clone方法。

```js
let r = new Request(`url`);

fetch(r.clone());
fetch(r);
```


# Response对象
Response对象是获取资源响应的接口。这个接口暴露了响应的相关信息，也暴露了使用响应体的不同方式。

## 创建Response对象
和前面几个对象一样，可以用构造函数的方法创建Response对象，且该构造函数不需要必须参数，此时Response实例的属性均为默认值，因为它并不代表实际的HTTP响应。

Response构造函数接收一个可选的body参数，这个body可以是null，等同于init对象中的body，还可以接收一个可选的init对象，该init对象应有三个键值对

        headers 必须是Headers对象实例
        status 表示HTTP的状态码
        statusText 表示HTTP响应状态的字符串

但大部分情况下，产生Response对象的主要方式还是调用fetch(),fetch()返回一个最终会成为Response对象的Promise实例，

还有两种生成Response对象的静态方法，分别是Response.redirect()和Response.error()，前者接收一个URL和一个重定向状态码，(301,302...),返回重定向的Response对象:

```js
Response.redirect(`url`,301)
```
后者则用于产生表示网络错误的Response对象，(网络错误会导致fetch()期约被拒绝).

Response对象只包含一组只读属性，描述了完成后的状态
```js
headers: //响应包含的Headers对象
ok: //布尔值，表示HTTP状态码的含义
redirected: //布尔值，表示响应是否至少经过一次定向
status: //整数，表示响应的HTTP状态码
statusText: //字符串，对HTTP状态码的描述
type: //字符串，包含响应类型。分为basic/cors/error/opaque/opaqueredirect
url: //包含响应URL的字符串

```

## 克隆Response对象
就是使用clone()方法呀，创建一个一摸一样的副本，和Request对象的clone差不多，该方法也主要用于多次读取Response对象，Response对象是只可以读取一次的。

## Request、Response、Body的混用
这部分内容涉及到Body的五个方法，用于将ReadableStream转存到缓冲区的内存里，这部分也没太懂，可能是浏览器机制和HTTP没有过多了解的缘故，打算重看一遍图解HTTP，所以这部分笔记就先不写了，有空再补，

## 一次性流
因为Body混入是构建在ReadableStream之上的，所以主体流只能使用一次。这也是为什么要用clone方法的原因。

## 使用ReadableStream主体
这部分也后续再补吧，实在没怎么看懂....