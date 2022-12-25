---
title: login
date: 2022-12-25 22:22:43
tags: [业务,登录]
categories: [后端,业务]
---

## 单一服务模式 登录

### 使用session对象实现

* 登录成功后，把用户数据放到session中
* 判断是否登录，从session中获取数据，可以获取到登录

具有局限性，只能在一台服务器上实现，单点性能压力大，无法扩展

<!--more-->

## 单点登录模式 （SSO模式）

* session广播机制实现
  * session复制到其他模块中，实现登录机制
* cookie + redis实现
  * 登录后数据放入两个地方
    * cookie：redis生成的key放入cookie
    * redis：key:生成唯一随机值，value:用户数据
  * 实现原理
    * 浏览器发送请求带着cookie进行发送，获取cookie,拿着cookie去redis根据key查询，查到数据登录
* token实现
  * 按照一定规程生成一个字符串，一般拥有用户信息
    * 一般使用base64加密
  * 生成字符串后将字符串返回
    * 可以使用cookie和地址栏进行传输

## JWT 工具

自包含令牌

使用jwt规则生成字符串

JWT包含三部分

* jwt头信息
  * 描述jwt元数据的json对象
* 有效载荷
  * 包含主体信息（用户信息）
* 签名哈希 防伪标志
  * 判定是否由我们规则生成字符串
