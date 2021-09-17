---
title: antd的基本使用
date: 2021-05-12 21:24:33
tags: project
categories: daily
---

# 引入组件
先引入antd库嘛
`yarn add antd`

然后想用什么组件就去官网找相应代码，比如有一个Button按钮想引入，就直接import导入
`import {Button} from 'antd'`
在你想要的合适的位置直接放进去就行
`<Button type="primary">Primary Button</Button>`
<!-- more -->
用的比较多的是图标哈，就需要有另一个库
`yarn add @ant-design/icons`
想要啥就import导入然后写下来就行
```js
import { WechatOutlined, WeiboOutlined} from '@ant-design/icons'

<WechatOutlined />
<WeiboOutlined />
```
还有轮播图啥的巴拉巴拉，自己去看官方文档就好了。

# 按需引入和自定义内容

这个好好看官方文档吧，3版本和4版本的不一样，暂时还没弄懂。。。。