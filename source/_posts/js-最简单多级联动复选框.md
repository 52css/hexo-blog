---
title: js-最简单多级联动复选框
date: 2019-07-09 09:12:57
tags: [js,multi,cascade,checkbox]
---
### 效果图

![预览效果图](https://i.loli.net/2019/07/09/5d23ead3963e118654.gif)

### 实现代码
```html
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <title>最简单多级联动复选框</title>
  <!-- 引入样式 -->
  <link rel="stylesheet" href="https://unpkg.com/element-ui/lib/theme-chalk/index.css">
</head>
<body>
  <div id="app">
    <ul v-for="one in tree" :key="one.code">
      <li>
        <el-checkbox :indeterminate="one.isIndeterminate" v-model="one.checked" @change="handleClick(one.code, one.checked)">
          {{ one.region }}
        </el-checkbox>
        <ul v-for="two in one.containerCitys" :key="two.code">
          <li>
            <el-checkbox :indeterminate="two.isIndeterminate" v-model="two.checked" @change="handleClick(two.code, two.checked)">
              {{ two.region }}
            </el-checkbox>
            <ul>
              <li v-for="three in two.containerCitys" :key="three.code">
                <el-checkbox
                  v-model="three.checked"
                  :key="three.code"
                  :indeterminate="three.isIndeterminate"
                  @change="handleClick(three.code, three.checked)">
                  {{ three.region }}
                </el-checkbox>
              </li>
            </ul>
          </li>
        </ul>
      </li>
    </ul>
  </div>
  <script src="https://cdn.jsdelivr.net/npm/vue/dist/vue.js"></script>
  <!-- 引入组件库 -->
  <script src="https://unpkg.com/element-ui/lib/index.js"></script>
  <script>
    var data = [
      {
        code: '01',
        region: '华东',
        containerCitys: [
          {
            region: '上海',
            code: '001',
            containerCitys: [
              {
                code: '0011',
                region: '徐家汇'
              },
              {
                code: '0012',
                region: '浦东新区'
              }
            ]
          },
          {
            region: '河南',
            code: '002',
            containerCitys: [
              {
                code: '0021',
                region: '郑州'
              },
              {
                code: '0022',
                region: '洛阳'
              },
              {
                code: '0023',
                region: '驻马店'
              }
            ]
          },
          {
            region: '河北',
            code: '003',
            containerCitys: [
              {
                code: '0031',
                region: '石家庄'
              },
              {
                code: '0032',
                region: '唐山'
              }
            ]
          },
          {
            region: '山东',
            code: '004',
            containerCitys: [
              {
                code: '0041',
                region: '济南'
              },
              {
                code: '0042',
                region: '青岛'
              }
            ]
          },
          {
            region: '浙江',
            code: '005',
            containerCitys: [
              {
                code: '0051',
                region: '杭州'
              },
              {
                code: '0052',
                region: '台州'
              }
            ]
          },
          {
            region: '江苏',
            code: '006',
            containerCitys: [
              {
                code: '0061',
                region: '南京'
              },
              {
                code: '0062',
                region: '苏州'
              }
            ]
          },
          {
            region: '北京',
            code: '007',
            containerCitys: [
              {
                code: '0071',
                region: '东城区'
              },
              {
                code: '0072',
                region: '通州区'
              }
            ]
          },
          {
            region: '山西',
            code: '008',
            containerCitys: [
              {
                code: '0081',
                region: '西安'
              },
              {
                code: '0082',
                region: '大同'
              }
            ]
          },
          {
            region: '天津',
            code: '009',
            containerCitys: []
          }

        ]
      },
      {
        code: '02',
        region: '华中',
        containerCitys: [
          {
            region: '湖北',
            code: '021',
            containerCitys: [
              {
                code: '0211',
                region: '武汉'
              },
              {
                code: '0212',
                region: '武昌'
              }
            ]
          },
          {
            region: '湖南',
            code: '022',
            containerCitys: [
              {
                code: '0221',
                region: '长沙'
              },
              {
                code: '0222',
                region: '娄底'
              }
            ]
          },
          {
            region: '安徽',
            code: '023',
            containerCitys: [
              {
                code: '0231',
                region: '合肥'
              },
              {
                code: '0232',
                region: '芜湖'
              }
            ]
          }
        ]
      }
    ]

    function tree2arr(tree, id = 'id', children = 'children') {
      const rtv = []

      loop(tree, 0)

      return rtv

      function loop(arr, parentId) {
        arr.forEach((item) => {
          item.parentId = parentId
          rtv.push(item)
          if (item[children] && item[children].length) {
            loop(item[children], item[id])
          }
        })
      }
    }

    function arr2tree(arr, id = 'id', children = 'children') {
      return loop(0)

      function loop(parentId) {
        const rtv = []
        arr.forEach((item) => {
          if (item.parentId === parentId) {
            const childrenList = loop(arr, item[id], id, children)

            if (childrenList && childrenList.length) {
              item[children] = childrenList
            }
            rtv.push(item)
          }
        })
        return rtv
      }
    }

    function Event() {
      this.handlers = {};
    }
    Event.prototype = {
        // 订阅事件
        on: function(eventType, handler){
            var self = this;
            if(!(eventType in self.handlers)) {
               self.handlers[eventType] = [];
            }
            self.handlers[eventType].push(handler);
            return this;
        },
         // 触发事件(发布事件)
        emit: function(eventType){
           var self = this;
           var handlerArgs = Array.prototype.slice.call(arguments,1);
           for(var i = 0; i < self.handlers[eventType].length; i++) {
             self.handlers[eventType][i].apply(self,handlerArgs);
           }
           return self;
        },
        // 删除订阅事件
        off: function(eventType, handler){
            var currentEvent = this.handlers[eventType];
            var len = 0;
            if (currentEvent) {
                len = currentEvent.length;
                for (var i = len - 1; i >= 0; i--){
                    if (currentEvent[i] === handler){
                        currentEvent.splice(i, 1);
                    }
                }
            }
            return this;
        }
    };

    var app = new Vue({
      el: '#app',
      data() {
        return {
          arr: []
        }
      },
      computed: {
        tree() {
          return arr2tree(this.arr, 'code', 'containerCitys');
        }
      },
      mounted() {
        const self = this;
        const flatArr = tree2arr(data, 'code', 'containerCitys');

        // bind event
        flatArr.forEach((item, index) => {
          var event = new Event();

          item.$event = event

          item.$event.on('checked', function(val) {
            // 影响自己
            item.checked = val;
            item.isIndeterminate = false;

            // 影响子
            item.$event.emit('children-checked', val);

            // 影响父
            item.$event.emit('parent-checked');
          });

          item.$event.on('children-checked', function(val) {
            // 影响子
            if (item.containerCitys) {
              item.containerCitys.forEach(x => {
                x.checked = val;
                x.isIndeterminate = false;
                x.$event.emit('children-checked', val);
              })
            }
          });

          item.$event.on('parent-checked', function() {
            let [ parent ] = flatArr.filter(x => x.code === item.parentId);

            if (parent) {
              const allChecked = parent.containerCitys.every(x => x.checked);
              const hasChecked = parent.containerCitys.some(x => x.checked || x.isIndeterminate)

              parent.checked = allChecked;
              parent.isIndeterminate = !allChecked && hasChecked;

              parent.$event.emit('parent-checked');
            }
          });
        });

        this.arr = flatArr;
      },
      methods: {
        handleClick(id, val) {
          this.clickTreeById(id, val);
          this.$forceUpdate();
        },
        clickTreeByIndex(index, val) {
          const [item] = this.arr[index];

          if (item) {
            item.$event.emit('checked', val);
          }
        },
        clickTreeById(id, val) {
          const [ item ] = this.arr.filter(x => x.code === id)

          if (item) {
            item.$event.emit('checked', val);
          }
        }
      }
    });
  </script>
</body>
</html>
```