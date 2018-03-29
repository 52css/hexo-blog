---
title: 【特效】瓶中摇摆的水
date: 2017-09-13 22:53:48
tags: [effect,aniamte,water,bottle]
---
 如果只是摇摆的水，可以一张长图，通过css移动background-positon来实现
```css
.water {
  background: url('./water.png');
  background-position:0 0;
  -webkit-animation:bpa 3s infinite linear ;
}
@-webkit-keyframes bpa {
  0%{
    background-position: 0 0;
  }
  100%{
    background-position: -3627px 0;
  }
}
```
 现在难在不规则形状瓶子中显示，本能想到裁剪成不规则就行，查阅css有clip-path属性，可以设置成clip-path: url(#clipPath); svg对应裁剪标签元素id
```html
<?xml version="1.0" encoding="utf-8"?>
<!-- Generator: Adobe Illustrator 19.2.1, SVG Export Plug-In . SVG Version: 6.00 Build 0)  -->
<svg version="1.1" id="图层_1" xmlns="http://www.w3.org/2000/svg" xmlns:xlink="http://www.w3.org/1999/xlink" x="0px" y="0px"
 viewBox="0 0 523.6 1437.1" style="enable-background:new 0 0 523.6 1437.1;" xml:space="preserve">
<style type="text/css">
.st0{fill:#FFFFFF;}
</style>
<title>瓶子</title>
<clipPath id="clipPath">
<path class="st0" d="M501.8,1227.1c-26.6-64.6-20.1-133.6-10.2-199.7c9.9-66.1,22.6-175,20.4-184.3c-2.2-9.3-5.9-8.2,0-41
    c5.9-32.8-3.6-185.1-10.2-204.8s-15.2-24.5-15.3-41c-1.3-33.2-69.3-209.7-84.5-259.3s-11.5-106.8-11-124c21.4-23.4,20.7-38.5,18-57
    s-13.8-34.9-1-43s18.5-50.8-27.9-62c-8.9-2.1-71.2-9.8-90.1-10L252,0h-5.5h-4.8L202,1.1c-21.1,0.2-58.9,5.7-67.2,10
    c-31.7,16.8-29.5,41.5-20.8,54c20.7,29.4,3,32.4,0.3,50.9s-1,33,20.5,56.3c0.5,17.3,5.1,73.3-10.3,122.9s-85.9,228-87.2,261.1
    c-0.1,16.5-8.7,21.3-15.4,41s-16.2,172-10.3,204.8c5.9,32.8,2.2,31.6,0,41c-2.2,9.4,10.5,118.2,20.5,184.3
    c10,66.1,16.5,135.1-10.3,199.7s-29.6,125.3-5.1,189.4c3.5,9.4,13.6,13.2,30.8,15.4c15.5,1.9,172.1,4.5,199.3,5v0.1h2.8h2.3v-0.1
    c23.7-0.4,162.9-2.1,192.7-5c17.1-1.7,58.7-5.9,62.2-15.4C531.3,1352.5,528.4,1291.7,501.8,1227.1z"/>
</clipPath>
</svg>
```

### 完整的代码

#### html代码
```html
<div class="bottle-wrap">
    <?xml version="1.0" encoding="utf-8"?>
    <!-- Generator: Adobe Illustrator 19.2.1, SVG Export Plug-In . SVG Version: 6.00 Build 0)  -->
    <svg version="1.1" id="图层_1" xmlns="http://www.w3.org/2000/svg" xmlns:xlink="http://www.w3.org/1999/xlink" x="0px" y="0px"
         viewBox="0 0 523.6 1437.1" style="enable-background:new 0 0 523.6 1437.1;" xml:space="preserve">
    <style type="text/css">
        .st0{fill:#FFFFFF;}
    </style>
    <title>瓶子</title>
    <clipPath id="clipPath">
        <path class="st0" d="M501.8,1227.1c-26.6-64.6-20.1-133.6-10.2-199.7c9.9-66.1,22.6-175,20.4-184.3c-2.2-9.3-5.9-8.2,0-41
            c5.9-32.8-3.6-185.1-10.2-204.8s-15.2-24.5-15.3-41c-1.3-33.2-69.3-209.7-84.5-259.3s-11.5-106.8-11-124c21.4-23.4,20.7-38.5,18-57
            s-13.8-34.9-1-43s18.5-50.8-27.9-62c-8.9-2.1-71.2-9.8-90.1-10L252,0h-5.5h-4.8L202,1.1c-21.1,0.2-58.9,5.7-67.2,10
            c-31.7,16.8-29.5,41.5-20.8,54c20.7,29.4,3,32.4,0.3,50.9s-1,33,20.5,56.3c0.5,17.3,5.1,73.3-10.3,122.9s-85.9,228-87.2,261.1
            c-0.1,16.5-8.7,21.3-15.4,41s-16.2,172-10.3,204.8c5.9,32.8,2.2,31.6,0,41c-2.2,9.4,10.5,118.2,20.5,184.3
            c10,66.1,16.5,135.1-10.3,199.7s-29.6,125.3-5.1,189.4c3.5,9.4,13.6,13.2,30.8,15.4c15.5,1.9,172.1,4.5,199.3,5v0.1h2.8h2.3v-0.1
            c23.7-0.4,162.9-2.1,192.7-5c17.1-1.7,58.7-5.9,62.2-15.4C531.3,1352.5,528.4,1291.7,501.8,1227.1z"/>
    </clipPath>
    </svg>

    <div class="water-wrap">
        <div class="water"></div>
    </div>
    <img src="可乐瓶.png" class="bottle">
</div>
```
#### css代码
```css
body {
    background-color: #000;
}
.bottle-wrap {
    width: 524px;
    height: 1438px;
    position: relative;
}
.water-wrap {
    position: absolute;
    top: 0;
    left: 0;
    right: 0;
    bottom: 0;
    clip-path: url(#clipPath);
}
.water {
    position: absolute;
    bottom: 0;
    left: 0;
    right: 0;
    height: 50%;
    /* background-color: yellow; */
    background: url('./water.png');
    background-position:0 0;

    -webkit-animation:bpa 3s infinite linear ;
}

@-webkit-keyframes bpa {
    0%{
        background-position: 0 0;
    }
    100%{
        background-position: -3627px 0;
    }
}
.bottle {
    position: absolute;
    top: 0;
    left: 0;
}
```
 很久没用使用hexo，忘记怎么传递图片，暂时只有代码
