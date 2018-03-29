---
title: effect-svg-flipX
date: 2017-09-26 09:30:54
tags: [effect,svg,flipX]
---

svg贴图是根据svg标签插入image标签，设置href来显示内容
```html
<svg>
  <image href="http://www.baidu.com" />
</svg>
```
工作中需要对图片水平翻转，查看svg有transform 属性，和css类似，但是不一样，css翻转代码
```css
img {
  -moz-transform: scaleX(-1);
  -o-transform: scaleX(-1);
  -webkit-transform: scaleX(-1);
  transform: scaleX(-1);
  filter: FlipH;
  -ms-filter: "FlipH";
}
```
查下来，svg翻转需要 scale(-1,1) 来，例如你坐标是（100,100） 通过翻转变成 （-100,100），而左边位置是图片左上角位置

### 产看react部分代码
```html
<div className="cp-svg-flip">
  <svg version="1.1" xmlns="http://www.w3.org/2000/svg" x="0px" y="0px" viewBox="0 0 523.6 1437.1">
    <image ref="img1" y="0" width="27" height="40" href={moveSrc}></image>
    <image ref="img2" y="40" width="27" height="40" href={moveSrc}></image>
  </svg>
</div>
```

```js
let img1 = this.refs.img1,
  img2 = this.refs.img2,
  posX = 100, // 希望位置
  trueX = posX - 27 / 2; // 默认是顶点对齐，改成x轴中心对齐

img1.setAttribute('x', trueX); // 设置x位置
img2.setAttribute('x', trueX); // 设置x位置

img2.setAttribute(
  'transform',
  'translate(' + (posX * 2) + ',0) scale(-1,1)' // 水平翻转
);
```

### [查看demo](/effect/#/svg-flip)
