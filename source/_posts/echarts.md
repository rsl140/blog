---
title: vue中使用echarts 使用记录
date: 2019-01-16 16:52:19
tags: vue
categories: FE
keywords: echarts
---
> 仅以此记录下个人使用echart的方法，毕竟好记性不如烂笔头。


# 参考资料
[手摸手，带你用vue撸后台 系列三(实战篇)](https://segmentfault.com/a/1190000009762198)

# 安装echarts
`npm install echarts --save`
# echarts package版本
`"echarts": "^4.2.0-rc.2"`
<!--  more  -->

# utils
``` javascript
export function debounce (func, wait, immediate) {
  let timeout, args, context, timestamp, result

  const later = function () {
    // 据上一次触发时间间隔
    const last = +new Date() - timestamp

    // 上次被包装函数被调用时间间隔last小于设定时间间隔wait
    if (last < wait && last > 0) {
      timeout = setTimeout(later, wait - last)
    } else {
      timeout = null
      // 如果设定为immediate===true，因为开始边界已经调用过了此处无需调用
      if (!immediate) {
        result = func.apply(context, args)
        if (!timeout) context = args = null
      }
    }
  }

  return function (...args) {
    context = this
    timestamp = +new Date()
    const callNow = immediate && !timeout
    // 如果延时不存在，重新设定延时
    if (!timeout) timeout = setTimeout(later, wait)
    if (callNow) {
      result = func.apply(context, args)
      context = args = null
    }

    return result
  }
}
```

# chart组件代码
``` javascript
<template>
  <div :class="className" :style="{height:height,width:width}"></div>
</template>

<script>
import echarts from 'echarts'
import { debounce } from '@/utils'

export default {
  props: {
    className: {
      type: String,
      default: 'chart'
    },
    width: {
      type: String,
      default: '100%'
    },
    height: {
      type: String,
      default: '350px'
    },
    autoResize: {
      type: Boolean,
      default: true
    },
    chartData: {
      type: Object
    }
  },
  data () {
    return {}
  },
  mounted () {
    this.initChart()
    if (this.autoResize) {
      this.__resizeHanlder = debounce(() => {
        if (this.$chart) {
          this.$chart.resize()
        }
      }, 100)
      window.addEventListener('resize', this.__resizeHanlder)
    }
  },
  beforeDestroy () {
    if (!this.$chart) {
      return
    }
    if (this.autoResize) {
      window.removeEventListener('resize', this.__resizeHanlder)
    }

    this.$chart.dispose()
    this.$chart = null
  },
  watch: {
    chartData: {
      deep: true,
      handler (val) {
        this.setOptions(val)
      }
    }
  },
  methods: {
    setOptions (val) {
      this.$chart.clear() // 清空画布
      this.$chart.setOption(val)
    },
    initChart () {
      this.$chart = echarts.init(this.$el, 'macarons')
      this.setOptions(this.chartData)
    }
  }
}
</script>

```
# 使用方法
``` javascript
<template>
    <div>
        <charts :chartData="Option"></charts>
    </div>
</template>

<script>
import Charts form '@/components/Charts'

export default {
  name: 'chartDemo',
  components: {
    Charts
  },
  data () {
    return {
      Option: {}
    }
  },
  mounted () {
    this.getList()
  },
  methods: {
    getList () {
        this.Option = {
            xAxis: {
                type: 'category',
                data: ['Mon', 'Tue', 'Wed', 'Thu', 'Fri', 'Sat', 'Sun']
            },
            yAxis: {
                type: 'value'
            },
            series: [{
                data: [820, 932, 901, 934, 1290, 1330, 1320],
                type: 'line'
            }]
        }
    }
  }
</script>
```
