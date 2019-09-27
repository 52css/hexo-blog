---
title: Vue 高阶组件element-ui button
date: 2019-09-27 15:05:30
tags: [vue,hoc,element-ui,button]
---

```html
<script>
export default {
  name: 'ButtonHoc',
  props: {},
  data() {
    return {
      loading: false // 显示加载中
    }
  },
  methods: {
    handleClickLink(event) {
      const that = this
      const fn = that.$listeners.click
      const timer = that.$attrs.timer || 300
      this.loading = true

      event.preventDefault()

      // 如果是promise，等有结果关闭loading
      try {
        fn.call(that, event)
          .then(() => {
            this.loading = false
          })
          .catch(() => {
            this.loading = false
          })
      } catch (ex) {
        // 延迟300ms，关闭loading
        setTimeout(() => {
          this.loading = false
        }, timer)
      }
    }
  },
  render(h) {
    // 内容插槽
    const slots = Object.keys(this.$slots)
      .reduce((arr, key) => arr.concat(this.$slots[key]), [])
      .map(vnode => {
        vnode.context = this._self
        return vnode
      })

    return h('el-button', {
      on: {
        click: this.handleClickLink // 新组件绑定click事件
      },
      props: this.$props,
      // 透传 scopedSlots
      scopedSlots: this.$scopedSlots,
      attrs: {
        ...this.$attrs,
        loading: this.loading // 显示loading
      }
    }, slots)
  }
}
</script>

<style>

</style>

```
