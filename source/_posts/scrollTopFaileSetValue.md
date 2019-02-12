---
title: vue scrollTop 无法赋值
date: 2018-12-21 16:48:36
tags: vue
categories: FE
keywords: scrollTop
---
> vue scrollTop 无法赋值

## 遇到问题

`container.scrollTop 一直为0`不能赋值

``` javascript
  watch: {
    historyList () {
      this.$nextTick(() => {
        const container = this.$el.querySelector('.scrolldivmain')
        console.log(container.scrollHeight)
        console.log(container.scrollTop)
        this.$refs.scrolldiv.scrollTo(0, container.scrollHeight + 'px')
        container.scrollTop = container.scrollHeight
        container.scrollTop(0, container.scrollHeight)
        console.log(container.scrollTop) // container.scrollTop 一直为0
      })
    }
  }
```
<!--  more  -->

## 注意点
 - 确定下滚动条是在哪里显示的
  有个方法判断下：
``` javascript
    window.addEventListener('scroll', () => {
        var scrollTop = this.$el.querySelector('.scrolldivmain')
        // console.log(scrollTop.scrollHeight)
        console.log(scrollTop.scrollTop) // 查看打印的值是否有变化 如果有 则说明滚滚动条在这个标签中
        // scrollTop.scrollTop = scrollTop.scrollHeight // 可以尝试下 滚动滚动条。一直在底部则可以设置成功
    }, true)
```

## 解决方案
需要用到的地方 调用this.scrollToBottom()即可
``` html
<!--element-ui-->

<el-main class="scrolldivmain">
    some code
</el-main>
```

``` javascript
methods: {
  // 滚动到底部
    scrollToBottom () {
      this.$nextTick(() => {
        setTimeout(() => {
          var scrollTop = this.$el.querySelector('.scrolldivmain')
          scrollTop.scrollTop = scrollTop.scrollHeight
        }, 13)
      })
    }
}
```

## 参考文章
[vue 无法监听scroll事件](https://segmentfault.com/q/1010000009119633)
[参考vue-chat代码](https://github.com/yiluxiangbei87110/vue-chat)
