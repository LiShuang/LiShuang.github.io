---
layout: post
title: About HTTPS
category: other
comments: false
description: 
tags:
    - https
---

## HTTPS  
### 关于证书  
按认证级别划分：  
* 域名认证：最低级别认证，浏览器地址栏会显示一把锁  
* 公司认证  
* 扩展认证：最高级别认证，浏览器地址栏会显示公司名称  

按覆盖范围划分：  
* 单域名证书：只能用于单一域名  
* 通配符证书：可以用于某个域名及其一级子域名  
* 多域名证书：用于多个域名  

### 修改资源链接  
网页加载的 http 资源，要全部改成 https 的链接。例如：  

    <script src="http://test.youdao.com/jquery.js"></script>  
    
可修改为：  

    // 方法一
    <script src="https://test.youdao.com/jquery.js"></script>
    
    // 方法二（根据当前网页的协议，加载相同协议的外部资源）  
    <script src="//test.youdao.com/jquery.js"></script>

若页面头部引用了 rel="canonical" 的资源，也需要改成 HTTPS 。例如： 

    <link rel="canonical" src="https://test.youdao.com/test.html">  

### 301重定向   

修改服务器的配置文件，使用 301 重定向。  

Nginx 的写法：  

    server {
      listen 80;
      server_name domain.com www.domain.com;
      return 301 https://domain.com$request_uri;
    }  

Apache 的写法（.htacess文件）：  
    
    RewriteEngine On
    RewriteCond %{HTTPS} off
    RewriteRule (.*) https://%{HTTP_HOST}%{REQUEST_URI} [R=301,L]

### 安全措施  
#### HSTS  

HTTP 严格传输协议(HTTP Strict Transport Security)，即强制浏览器只能发出 HTTPS 请求，并阻止用户接受不安全的证书。  

在网页的响应头里加入一个强制性声明：  

    Strict-Transport-Security: max-age=31536000; includeSubDomains

缺点是：用户首次访问网站发出 HTTP 请求时，是不受 HSTS 保护的。  

### Cookie  
确保浏览只有在使用 HTTPS 时，才发送 Cookie。可在网页响应头的 Set-Cookie 字段加上 Secure 标志。  

    Set-Cookie: LSID=DQAAAK...Eaem_vYg; Secure  
