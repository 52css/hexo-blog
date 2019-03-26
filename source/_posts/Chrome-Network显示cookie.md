---
title: Chrome-Network显示cookie
date: 2019-03-26 09:23:46
tags: [chrome,network,cookie]
---

### Chrome开发者工具中看不到网络请求的cookie信息

Chrome67引入饿一个新特性：site-isolation （站点隔离），也就是请求第三方站点数据时，如cookie等敏感信息会被隐藏显示为”Provisional headers are shown”

如果需要禁用站点隔离可以在chrome://flags配置

解决方法：
1.访问chrome://flags/#site-isolation-trial-opt-out

2. 选择Opt-out

3. 重启浏览器，完成。
