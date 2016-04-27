---
title: require.js
date: 2016-04-18 15:51:53
tags: require.js
---
>   require.js 是 AMD 规范实现

### 入口
```
<script data-main="scripts/main.js" src="scripts/require.js"></script>
```
### baseUrl
如果没有显式指定config及data-main，则默认的baseUrl为**包含RequireJS**的那个***HTML***页面的所属目录。
如果用了data-main属性，则该路径就变成baseUrl。
 <!-- more -->

### 定义模块
>   简单值对
```
    define({
        color: "black",
        size: "unisize"
    });
```

>   无依赖函数式
```
    define(function () {
       return {
           color: "black",
           size: "unisize"
       }
    });
```