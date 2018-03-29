---
title: 【特效】前端实现弹幕效果
date: 2017-09-16 16:44:38
tags: [react,video,barrage,effect]
---

### [查看demo](/effect/#/video-barrage)

 弹幕是根据视频来播放，另外一组弹幕数据过来负责显示，这样弹幕组件需要2个数据，还有一个添加弹幕方法
 1. 视频源
 2. 弹幕数据
 3. 添加弹幕方法

```html
<VideoBarrage
   videoSrc={videoSrc}
   barrageList={this.state.barrageList}
   addBarrage={(obj) => this.handleAddBarrage(obj)}
 />
```

### 整个父页面
```javascript
import React from 'react';
 import VideoBarrage from 'components/VideoBarrage';

 class VideoBarrageComponent extends React.Component {
   /**
    * [constructor 初始化弹幕]
    * @param  {[type]} props [description]
    * @return {[type]}       [description]
    */
   constructor(props) {
     super(props);
     this.state = {
       barrageList: [{
         value: 'speed设为0为非滚动',
         time: 1, // 单位秒
         speed: 0
       }, {
         value: 'time控制弹幕时间，单位秒',
         color: 'blue',
         time: 2,
         speed: 2
       }, {
         value: '视频共21秒',
         time: 3.2,
         speed: 3
       }, {
         value: '视频背景为白色',
         time: 4.5
       }, {
         value: '视频为录制',
         time: 5.0
       }, {
         value: '视频内容简单',
         time: 6.3
       }, {
         value: '是为了让视频尺寸不至于过大',
         time: 7.8
       }, {
         value: '省流量',
         time: 8.5
       }, {
         value: '支持弹幕暂停（视频暂停）',
         time: 9
       }, {
         value: 'add()方法新增弹幕',
         time: 11
       }, {
         value: 'reset()方法重置弹幕',
         time: 11
       }, {
         value: '颜色，字号，透明度可全局设置',
         time: 13
       }, {
         value: '具体交互细节可参考页面源代码',
         time: 14
       }, {
         value: '内容不错哦！',
         time: 18,
         color: 'yellow'
       }]
     }
   }

   /**
    * [handleAddBarrage 添加弹幕]
    * @param  {[type]} obj [description]
    * @return {[type]}     [description]
    */
   handleAddBarrage(obj) {
     let barrageList = this.state.barrageList;

     barrageList.push(obj);

     this.setState({
       barrageList
     })
   }

   /**
    * [render react-绚烂]
    * @return {[type]} [description]
    */
   render() {
     let videoSrc = require('./images/video.mp4');

     return (
       <div className="index">
         {this.state.barrageList.length}
         <VideoBarrage
           videoSrc={videoSrc}
           barrageList={this.state.barrageList}
           addBarrage={(obj) => this.handleAddBarrage(obj)}
         />
       </div>
     );
   }
 }

 VideoBarrageComponent.defaultProps = {
 };

 export default VideoBarrageComponent;
```

 子组件需要负责按需求绚烂弹幕内容，先讲讲怎么实现代码，就是canvas通过content.fillText显示下，显示就差运动，canvas运动不像div运动一样，设置top或left就可以移动，而是需要一直绘制来达到效果，所以每次绘制需要重新清除画布**context.clearRect(0, 0, canvasWidth, canvasHeight)**

### 子组件所有代码
```javascript
import React from 'react';
import './index.css';

class VideoBarrageComponent extends React.Component {
  constructor(props) {
      super(props);
      this.state = {
        canSubmit: false,
        value: '',
        opacity: 100,
        fontSize: 24,
        position: 'all',  //all 0,1 top 0,0.3 bottom 0.7,1
        color: '#ff0000'
      }
  }

  componentWillMount() {
  }

  /**
   * [componentDidMount dom加载完毕]
   * @return {[type]} [description]
   */
  componentDidMount() {
    let { videoSrc } = this.props,
      elVideo = this.refs.video,
      canvas = this.refs.barrage,
      self = this;

    // 给视频添加路径，通过父传递进来
    this.refs.video.src = videoSrc;

    // 给canvas重新设置宽高
    canvas.width = canvas.clientWidth;
    canvas.height = canvas.clientHeight;

    // 视频开始播放
    elVideo.addEventListener('play', () => {
      self.isPause = false;
      self.canvasRender();
      // self.addText('2134');
    });

    // 暂停播放
    elVideo.addEventListener('pause', () => {
      self.isPause = true;
    });

    // 拖拽视频
    elVideo.addEventListener('seeked', function () {
  		// 跳转播放需要清屏
  		self.reset();
  	});
  }

  /**
   * [reset 清楚屏，并且更新弹幕是否显示属性]
   */
  reset() {
    let canvas = this.refs.barrage,
      video = this.refs.video,
      context = canvas.getContext('2d'),
      canvasWidth = canvas.width,
      canvasHeight = canvas.height,
      time = video.currentTime,
      { barrageList } = this.props;

      // 画布清除
  		context.clearRect(0, 0, canvasWidth, canvasHeight);

      barrageList.forEach(function(barrage) {
        if (barrage) {
  				// 状态变化
  				barrage.disabled = false;
  				// 根据时间判断哪些可以走起
  				if (time < barrage.time) {
  					// 视频时间小于播放时间
  					// barrage.disabled = true;
  					barrage.inited = null;
  				} else {
  					// 视频时间大于播放时间
  					barrage.disabled = true;
  				}
  			}
      });
  }

  /**
   * [clearRect 清空canvas]
   * @return {[type]} [description]
   */
  clearRect() {
    let canvas = this.refs.barrage,
      context = canvas.getContext('2d'),
      canvasWidth = canvas.width,
      canvasHeight = canvas.height;

    context.clearRect(0, 0, canvasWidth, canvasHeight);
    context.globalAlpha = 1;
    context.fillStyle = '#fff';
    context.fillRect(0, 0, canvasWidth, canvasHeight);
  }

  /**
   * [draw 绘制canvas]
   * @return {[type]} [description]
   */
  draw() {
    let time = this.refs.video.currentTime,
      canvas = this.refs.barrage,
      canvasWidth = canvas.width,
      canvasHeight = canvas.height,
      position = this.state.position,
      range,
      context = canvas.getContext('2d'),
      metrics,
      textWidth,
      { barrageList } = this.props;

    if (position === 'all') {
      range = [0, 1];
    } else if (position === 'top') {
      range = [0, 0.3];
    } else if (position === 'bottom') {
      range = [0.7, 1];
    }

    barrageList.forEach((barrage) => {
      if (barrage && !barrage.disabled && time >= barrage.time) {
        let fontSize = barrage.fontSize || 24;
        metrics = context.measureText(barrage.value)
        textWidth = metrics.width;

        barrage.speed  = barrage.speed || 0;

        if (barrage.speed) {
  				// 随着字数不同，速度会有微调
  				barrage.speed = barrage.speed + barrage.value.length / 100;
  			}

        if (barrage.isInited) {
          barrage.x -= (barrage.speed / 10 + 1);
        } else {
          barrage.isInited = true;
          barrage.x = canvasWidth;
          barrage.y = range[0] * canvasHeight + (range[1] - range[0]) * canvasHeight * Math.random();

          if (barrage.y < fontSize) {
    				barrage.y = fontSize;
    			} else if (barrage.y > canvasHeight - fontSize) {
    				barrage.y = canvasHeight - fontSize;
    			}
        }

        if (barrage.x < -textWidth) {
          barrage.disabled = true;
        } else {
          this.addText(barrage);
        }
      }
    });
  }

  /**
   * [addText 给canvas绘制文字]
   * @param {[type]} obj [description]
   */
  addText(obj) {
    let canvas = this.refs.barrage;
    let context = canvas.getContext('2d');
    let fontSize = obj.fontSize || '24';
    let fontFace = 'serif';
    let textFillColor = obj.color || 'red';
    let textBaseline = 'middle';
    let textAlign = 'left';
    let fontWeight = 'normal';
    let fontStyle = 'normal';

    // 添加文本
    context.shadowColor = 'rgba(0,0,0,'+ (obj.opacity || this.state.opacity) +')';
    context.shadowBlur = 2;
    context.fillStyle = 'transparent';
    context.textBaseline = textBaseline;
    context.textAlign = textAlign;
    context.font = fontWeight + ' ' + fontStyle + ' ' + fontSize + 'px ' + fontFace;
    context.fillStyle = textFillColor;
    context.fillText(obj.value, obj.x, obj.y);
  }

  /**
   * [canvasRender canvas-一直绘制函数]
   * @return {[type]} [description]
   */
  canvasRender() {
    this.clearRect();
    this.draw();

    // 如果暂停，不用再绘制
    if (this.isPause === false) {
      requestAnimationFrame(() => {
        this.canvasRender();
      });
    }
  }

  /**
   * [handleChange 改变form原生，更新到state]
   * @param  {[type]} e   [description]
   * @param  {[type]} key [description]
   * @return {[type]}     [description]
   */
  handleChange(e, key) {
    this.setState({
      [key]: e.target.value
    });
    // debugger;
    // console.log(this.refs[key].value);
  }

  /**
   * [handleInput 文平输入控制，1 赋值state 2 更新按钮是否禁用]
   * @param  {[type]} e [description]
   * @return {[type]}   [description]
   */
  handleInput(e) {
    let val = e.target.value;

    this.setState({
      value: val
    });

    this.setState({
      canSubmit: !!val
    });
  }

  /**
   * [handleClick 添加弹幕，调用父方法]
   * @param  {[type]} e [description]
   * @return {[type]}   [description]
   */
  handleClick(e) {
    let { addBarrage } = this.props;

    e.preventDefault();

    if (addBarrage) {
      addBarrage({
        value: this.state.value,
        time: this.refs.video.currentTime,
        color: this.state.color
      });
    }

    this.setState({
      value: ''
    });
    this.setState({
      canSubmit: false
    });
  }

  /**
   * [render react 绚烂]
   * @return {[type]} [description]
   */
  render() {
    return (
      <div className="video-barrage">
        <canvas ref="barrage"></canvas>
        <video ref="video" width="640" height="384" controls></video>
        <form>
          <p>
            透明度(0-100)：
            <input
              ref="opacity"
              type="range"
              onChange={(e) => this.handleChange(e, 'opacity')}
              className="range"
              value={this.state.opacity}
              min="0"
              max="100" />
            文字大小(16-32)：
            <input
              ref="fontSize"
              type="range"
              className="range"
              value={this.state.fontSize}
              onChange={(e) => this.handleChange(e, 'fontSize')}
              min="16"
              max="32" />
          </p>
          <p>
            弹幕位置：
            <label className="radio-inline">
              <input
                checked={this.state.position === 'all'}
                onChange={(e) => this.handleChange(e, 'position')}
                name="position"
                type="radio"
                value="all"
                /> 全部位置
            </label>
            <label className="radio-inline">
              <input
                checked={this.state.position === 'top'}
                onChange={(e) => this.handleChange(e, 'position')}
                name="position"
                type="radio"
                value="top"/> 顶部
            </label>
            <label className="radio-inline">
              <input
                checked={this.state.position === 'bottom'}
                onChange={(e) => this.handleChange(e, 'position')}
                name="position"
                type="radio"
                value="bottom"/> 底部
            </label>
          </p>
          <p className="last">
            颜色：<input
                type="color"
                ref="color"
                value={this.state.color}
                onChange={(e) => this.handleChange(e, 'color')} />
            文字：
            <input
              onInput={e => this.handleInput(e)}
              value={this.state.value}
              className="ui-input"
              ref="value" required />
            <input
              type="submit"
              value="发送弹幕"
              onClick={e => this.handleClick(e)}
              disabled={!this.state.canSubmit} />
          </p>
        </form>
      </div>
    );
  }
}

VideoBarrageComponent.defaultProps = {
};

export default VideoBarrageComponent;
```
