---
title: effect-png-icon-color-fill
date: 2017-09-20 09:02:48
tags: [effect,png,icon,color,fill]
---

设计提供png图片，只是一种颜色，有些时候需要移上去变个颜色，这样网页就需要2张图，如果css能实现png图片换任意颜色多好，查看css有个**drop-shadow** 属性可以达到我们的目的，使用下来会和源颜色重叠，现在需要隐藏（不可见、但是能使用）本尊。


```html
<i class="icon"><i class="icon icon-del" style="filter: 'drop-shadow(20px 0 " + color + ")'"></i></i>
```

```css
.icon {
  display: inline-block;
  width: 20px; height: 20px;
  overflow: hidden;
}
.icon-del {
  background: url(./images/delete.png) no-repeat center;
}
.icon .icon {
  position: relative;
  left: -20px;
  border-right: 20px solid transparent;
}
```

里面icon是20px，以上代码修改成自己icon实际尺寸

### [查看demo](/effect/#/png-icon-color-fill)
