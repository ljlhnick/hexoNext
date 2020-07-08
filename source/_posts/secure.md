---
title: 前端安全方面的学习
categories:
  - 前端
tags:
  - 前端
---

<!--more-->

# 前端安全

前端安全防范对开发越来越重要

## XSS

中文名跨域脚本攻击。攻击者将可执行的代码注入到网页中，可分为持久型和非持久型。
持久型：攻击代码被服务端写入数据库，攻击危害性大。对于评论功能得防范持久型 XSS 攻击
非持久型：通过修改 url 参数方式加入攻击代码，诱导用户访问链接从而攻击。谷歌浏览器会自动防御

防御方法
转义字符：对于用户的输入永不信任，转义输入内容，特别是" <> / 还有"alert","eval","onload","onfocus","onerror","onclick"等字符串。
CSP：建立白名单，通过配置规则，明确告诉浏览器哪些外部资源可以加载执行，不符合的进行拦截

## CSRF

中文为跨站请求伪造。攻击者构造出一个后端请求地址，诱导用户点击或通过某种途径自动发起请求

防御方法
Token: 每次发请求都带 token，服务器验证 token 是否有效
验证 Referer：验证它来判断是否是第三方网站发起

## XSS 和 CSRF 的区别

1.XSS 不需要登录，CSRF 需用户登录获取其 cookie
2.XSS 是向网站注入、执行 JS 代码，篡改网站内容。CSRF 利用网站漏洞，去请求网站的 API。

## 点击劫持

是视觉欺骗。攻击者将攻击网站通过 iframe 嵌套的方式嵌入，并将 iframe 设置为透明，在页面中透出按钮诱导用户点击

防御方法
X-FRAME-OPTIONS 为一个 HTTP 响应头，可以防御 iframe 嵌套的点击劫持攻击
js 防御

## SQL 注入攻击

将 sql 命令插入表单，url，页面请求得传参中，从而欺骗服务器执行恶意 sql