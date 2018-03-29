---
title: angular1.0写组件
date: 2017-09-12 22:19:34
tags: [angular,component,directive]
---


### angular1 写组件

angular1.0 没有组件的概念，但是有指令完全可以完成组件开发，而vue区分和指令和组件，而react只有组件。先说说为什么需要写组件？记得很久前一个后端大神和我说过，程序分3个阶段，第一阶段，看得到别人代码，第二阶段，写得出需求代码，第三阶段，是写得出别人看得懂的代码。这样使程序很容易心浮气躁，到了第二阶段就以为很牛，久而久之，能写得出别人看得懂代码不易。程序往往把自己定位接盘侠，前面开发走了，还没有文档，代码还不易读懂。。。
扯远了，组件其实就是为了让页面模块化，从而简单化，而组件必须具备“函数式编程”，即指定传入参数，再有返回参数

```html
<hello-world></hello-world>
```

```javascript
angular.module('app')
  .directive('helloWorld', helloWorld);

/**
 * [helloWorld helloWorld指令]
 * @return {[type]} [description]
 */
function helloWorld() {
  return {
      restrict: 'EA', // E 代表 element 元素 A代表 attr 属性
      scope: { // 父传给子属性
      },
      template: '<div>Hello World</div>', // 子组件绚烂模板
      controller: helloWorldCtrl, // 子组件控制器
      controllerAs: 'vm', // 所以内部变量赋值到vm
      transclude: true, // 是否可以嵌套
      replace: true // 是否直接替换父元素
  };

  /**
   * [helloWorldCtrl helloWrold指令控制器]
   * @return {[type]} [description]
   */
  function helloWorldCtrl() {

  }
}
```

以上还是指令的普通用法，angular的scope可以
 * = 代表双绑
 * @ 单绑
 * & function

看是很好用 其实不然，
1. 双绑需要watch下，不让obect变化不起作用，
2. 单绑需要页面传递值{{val}}下，
3. 而函数如果父调用子方法，或者子调用父方法，函数的参数传递总有问题，有次偶尔的操作如果可以传递，
具体使用


### 父组件
```html
<my-input config="{
  oData: {
    val: vm.myVal
  },
  fnChange: vm.inputChange
  }"></my-input>
```
```javascript
vm.myVal = '123';
vm.inputChange = inputChange

/**
 * [inputChange 子组件会调用父组件方法，并且传递子组件参数]
 * @param  {[type]} val [description]
 * @return {[type]}     [description]
 */
function inputChange(val) {
  // 由于已经双绑，这里可以不用赋值，如果是单绑，需要给父组件值重新绑定下，如果是jq对象还需要$scope.$apply 赋值
  console.log(val);
  vm.myVal = val;
}
```
### 子组件
```javascript
angular.module('app')
  .directive('myInput', myInput);

/**
 * [myInput myInput组件]
 * @return {[type]} [description]
 */
function myInput() {
  return {
      restrict: 'EA', // E 代表 element 元素 A代表 attr 属性
      scope: { // 父传给子属性
        config: '='
      },
      template: '<input ng-model="vm.ngModel" ng-change="vm.config.fnChange(vm.ngModel)"/>', // 子组件绚烂模板
      controller: myInputCtrl, // 子组件控制器
      controllerAs: 'vm', // 所以内部变量赋值到vm
      transclude: true, // 是否可以嵌套
      replace: true // 是否直接替换父元素
  };

  /**
   * [myInputCtrl myInput指令控制器]
   * @return {[type]} [description]
   */
  function myInputCtrl() {
    let vm = this,
        unWatch;

    unWatch = $scope.$watch('config', fnWatchConfig, true);

    $scope.$on('$destroy', fnDestroy);

    /**
     * [fnDestroy 页面销毁]
     * @return {[type]} [description]
     */
    function fnDestroy() {
        unWatch();
    }

    /**
     * [fnWatchConfig 监控config]
     * @param  {[type]} newVal [description]
     * @return {[type]}        [description]
     */
    function fnWatchConfig(newVal) {
        vm.config = newVal;
        vm.ngModel = newVal.oData.val;
    }
  }
}
```

 这样指令可以想组件一样大胆写，但是毕竟是ng1.0，组件值类型不方便定义，写好组件再来个readme，描述出传递的类型，和所有参数

### 怎么合理拆分成组件
1. 公用区块（公共头部）
2. 公用特效（下拉框、单选框、复选框等）
3. bootstrap大模块拆分（表单、表格）

 这样下来大项目被合理拆分，而单独小组件只负责传入参数和传出指，想写乱代码都难

### 常用组件开发
Grid                     栅栏
Table                    表格
Form                     表单
Input                    输入框（text、emal、number）
DatePicker               日期选择器
Checkbox                 复选框
Radio                    单选框
Upload                   上传文件
Textarea                 多行输入框
Select                   下来选择框
Button                   按钮
Img                      图片形状(圆角、圆、缩略)
Text                     情境文本颜色（text-muted、text-primary、text-success、text-info、text-warning、text-danger）
BG                       情境背景色（bg-primary、bg-success、bg-info、bg-warning、bg-danger）
Span[close]              关闭按钮
Span[caret]              三角符号
Cascades                 级联选择器
Dropdown                 下拉菜单
Switch                   开关
Nav                      导航
Crumb                    面包屑（路径导航）
Modal                    确认弹框
Confirm                  确认弹框
Icon                     图标
ScrollView               滚动视图
Step                     步骤条
Tabs                     选项卡
Pagination               分页
Tag                      标签
Tips                     头部提示
Badge                    徽章
PageHeader               页头
Alert                    警告框
Progress                 进度条
巨幕
BtnGroup                 按钮组
InputGroup               输入框组
Thumbnail                缩略图
Media                    媒体对象（左右布局）
ListGroup                列表组
Panel                    面版
Well                     Well背景带圆角，带颜色

### 目的
1.公用代码只要写一次
2.修改同一代码只要一次

### 组件通信
最初用EventService 同步