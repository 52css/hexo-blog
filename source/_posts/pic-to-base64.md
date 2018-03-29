---
title: pic to base64
date: 2018-03-29 09:24:14
tags: [pic, base64]
---

### 图片转换base64

简单图片转base64，很简单, 加载图片，图片加载完毕，给canvas画图，再canvas.toDataURL();就行
```javascript
getBase64ByImg('', 100, 100)
  .then((canvas) => {
    console.log(canvas.toDataURL());
  });
/**
 * [getBase64ByImg description]
 * @param  {[type]} url    [description]
 * @param  {[type]} width  [description]
 * @param  {[type]} height [description]
 * @return {[type]}        [description]
 */
function getBase64ByImg(url, width, height) {
    return new Promise(function(resolve, reject) {
        var img = new Image();

        img.onload = function() {
            var canvas = document.createElement('canvas');
            var context = canvas.getContext('2d');
            // canvas绘制
            canvas.width = width;
            canvas.height = height;
            // 画布清除
            context.clearRect(0, 0, width, height);
            // 绘制图片到canvas
            context.drawImage(img, 0, 0);
            resolve(canvas);
        }

        img.onerror = reject;
        img.src = url;
    });
}
```
但是这个必须图片支持跨域，返回头包含
```
Access-Control-Allow-Origin:*
```
如果有些图片是第三方，需要二次封装，这样太麻烦，有没有什么可以直接转换？
```javascript
/*
 * @Author: weijie
 * @Date:   2018-03-28 21:32:52
 * @Last Modified by:   weijie
 * @Last Modified time: 2018-03-29 09:34:53
 */
/**
 * [getBase64 转换url为base64]
 * @param  {[type]} url [description]
 * @return {[type]}     [description]
 */
export default function getBase64(url) {
  return getAndEncode(url)
    .then((data) => {
      return dataAsUrl(data, mimeType(url));
    });
}

/**
 * [parseExtension 处理后缀]
 * @param  {[type]} url [description]
 * @return {[type]}     [description]
 */
function parseExtension(url) {
  const match = /\.([^.]*?)$/g.exec(url);
  if (match) {
    return match[1];
  }
  return '';
}
/**
 * [mimes 通用mime]
 * @return {[type]} [description]
 */
function mimes() {
  /*
   * Only WOFF and EOT mime types for fonts are 'real'
   * see http://www.iana.org/assignments/media-types/media-types.xhtml
   */
  const WOFF = 'application/font-woff';
  const JPEG = 'image/jpeg';

  return {
    woff: WOFF,
    woff2: WOFF,
    ttf: 'application/font-truetype',
    eot: 'application/vnd.ms-fontobject',
    png: 'image/png',
    jpg: JPEG,
    jpeg: JPEG,
    gif: 'image/gif',
    tiff: 'image/tiff',
    svg: 'image/svg+xml',
    mp4: 'video/mp4'
  };
}

/**
 * [mimeType 获取mime类型]
 * @param  {[type]} url [description]
 * @return {[type]}     [description]
 */
function mimeType(url) {
  const extension = parseExtension(url).toLowerCase();
  return mimes()[extension] || '';
}

/**
 * [dataAsUrl 转换base64]
 * @param  {[type]} content [description]
 * @param  {[type]} type    [description]
 * @return {[type]}         [description]
 */
function dataAsUrl(content, type) {
  return `data:${type};base64,${content}`;
}
/**
 * [getAndEncode 通过获取流形式来得到数据]
 * @param  {[type]} url [description]
 * @return {[type]}     [description]
 */
function getAndEncode(url) {
  const TIMEOUT = 30000;

  return new Promise((resolve) => {
    const request = new XMLHttpRequest();

    request.onreadystatechange = done;
    request.ontimeout = timeout;
    request.responseType = 'blob';
    request.timeout = TIMEOUT;
    request.open('GET', url, true);
    request.send();

    /**
     * [done description]
     * @return {Function} [description]
     */
    function done() {
      if (request.readyState !== 4) return;

      if (request.status !== 200) {
        fail(`cannot fetch resource: ${url}, status: ${request.status}`);

        return;
      }

      const encoder = new FileReader();
      encoder.onloadend = () => {
        const content = encoder.result.split(/,/)[1];
        resolve(content);
      };
      encoder.readAsDataURL(request.response);
    }

    /**
     * [timeout description]
     * @return {[type]} [description]
     */
    function timeout() {
      fail(`timeout of ${TIMEOUT}ms occured while fetching resource: ${url}`);
    }

    /**
     * [fail description]
     * @param  {[type]} message [description]
     * @return {[type]}         [description]
     */
    function fail(message) {
      resolve('');
    }
  });
}

```
### [查看demo](/effect/#/pic-to-base64)
