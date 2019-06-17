<template>
  <div v-if="!hidePage">
    <button @click="prevClick()" :class="{ active: nowPage === 1, disabled }" style="float:left">{{upName}}</button>
    <ul class="ul">
      <li v-if="leftDot" class="number" @click="skipFirstPage">1</li>
      <li v-if="leftDot" class="number" :style="{color:leftDotColor}" @click="quickSkip(false)" @mousemove="dotMove(false)"
        @mouseout="dotMoveOut(false)">
        {{leftDotVal}}</li>

      <li class="number" v-for="item in forItem" :key="item" :class="[item === nowPage?'lightNowPage':'']"
        @click="handleClick(item)"> {{item}}</li>
      <li v-if="rightDot" class="number" :style="{color:rightDotColor}" @click="quickSkip(true)" @mousemove="dotMove(true)"
        @mouseout="dotMoveOut(true)">
        {{rightDotVal}}</li>
      <li v-if="rightDot" class="number" @click="skipLastPage">{{pageCount}}</li>

    </ul>
    <button @click="nextClick()" :class="{ active: nowPage === pageCount, disabled }">{{downName}}</button>
  </div>

</template>

<script>
export default {
  name: 'Pagination',
  data () {
    return {
      nowPage: 1, // 当前页数
      disabled: false, // 是否禁用
      forItem: [], // 用于循环页码的数组
      rightDot: false, // 右省略号
      rightDotVal: '...',

      leftDot: false, // 左省略号
      leftDotVal: '...', // 左省略号显示的内容
      forItemFirst: 0, // 页码的第一个数字
      forItemLast: 0, // 页码的最后一个数字

      leftDotColor: '#606266',
      rightDotColor: '#606266',

      hideOnSinglePage: true, // 在页面只有一页的情况下是否隐藏
      hidePage: false // 是否隐藏页面

    }
  },
  created () {
    this.nowPage = this.currentPage
    this.selectForItem()
    this.hideSinglePage()
  },
  props: {
    upName: {
      type: String,
      default: '上一页'
    }, // 上一页显示按钮
    downName: {
      type: String,
      default: '下一页'
    }, // 下一页显示按钮
    pageCount: Number, // 总页数
    pageSize: {
      type: Number,
      default: 10
    }, // 每一页显示的条目
    currentPage: Number // 当前页
  },
  watch: {
    nowPage: function (newVal, oldVal) {
      if (newVal + this.pageSize / 2 + 1 > this.pageCount) { // 当前页面+pagesize的一半大于总页码就隐藏省略号
        this.rightDotVal = '...'
        this.rightDot = false
      } else {
        this.rightDot = true
      }
      if (newVal - this.pageSize / 2 - 1 <= 1) {
        this.leftDotVal = '...'
        this.leftDot = false
      } else {
        this.leftDot = true
      }
      // 当前页数如果大于pageSize/2时, 将整个页码往前移一位
      let halfPageCount = this.forItem[Math.floor(this.forItem.length / 2)] // 整个页码的中间一位
      let step = Math.abs(newVal - halfPageCount) // 如果是直接点击数字,页码跳转可以不止一位
      this.forItemFirst = this.forItem[0] - 1 // 页码的第一个数字
      this.forItemLast = this.forItem[this.forItem.length - 1] // 页码的最后一个数字
      if (newVal - oldVal > 0) { // 是否前进
        if (this.forItemLast + 1 > this.pageCount) { // 最后页码比总页数大
          this.forItemLast = this.pageCount // 将总页数赋给最后一个页码
        } else {
          let i = 0 // 用于在step很大时,每次push的值加一
          while (step--) {
            // 如果当前页码比大于7,则进行页码后移操作
            if (this.nowPage > Math.floor(this.pageSize / 2) + 1 && this.forItemLast + step < this.pageCount) { // 保证最后一个值不超过也没带总页数
              this.forItem.shift() // 去掉头部值
              this.forItem.push(this.forItemLast + 1 + i++)
            }
          }
        }
      } else {
        let i = 0 //  用于在step很大时,每次push的值加一
        while (step--) {
          if (this.forItemFirst - i >= 1 && this.nowPage <= this.pageCount - (Math.floor(this.pageSize / 2))) { // 防止页码小于1 , 保证页码在小于总页数的三个值之后才会前移
            this.forItem.pop() // 去掉尾部值
            this.forItem.unshift(this.forItemFirst - i++)
          }
        }
      }
    }
  },
  methods: {
    hideSinglePage () {
      this.pageCount === 1 && this.hideOnSinglePage ? (this.hidePage = true) : (this.hidePage = false)
    },
    skipLastPage () {
      this.nowPage = this.pageCount
    },
    skipFirstPage () {
      this.nowPage = 1
    },
    dotMove (isRight) {
      if (isRight) {
        this.rightDotColor = '#409eff'
        this.rightDotVal = '»'
      } else {
        this.leftDotColor = '#409eff'
        this.leftDotVal = '«'
      }
    },
    dotMoveOut (isRight) {
      if (isRight) {
        this.rightDotColor = '#606266'
        this.rightDotVal = '...'
      } else {
        this.leftDotColor = '#606266'
        this.leftDotVal = '...'
      }
    },
    selectForItem () {
      if (this.pageCount > this.pageSize) {
        for (let i = 1; i <= this.pageSize; i++) {
          this.forItem.push(i)
        }
        this.rightDot = true
      } else {
        for (let i = 1; i <= this.pageCount; i++) {
          this.forItem.push(i)
        }
      }
    },
    quickSkip (isRight) {
      if (isRight) {
        this.nowPage = this.nowPage + this.pageCount / 10
      } else {
        this.nowPage = this.nowPage - this.pageCount / 10
      }
    },
    prevClick () {
      if (this.nowPage - 1 < 1) {
        this.nowPage = 1
      } else {
        this.nowPage--
      }
      this.$emit('prev-click', this.nowPage)
    },
    handleClick (page) {
      this.nowPage = page
      this.$emit('click', page)
    },
    nextClick () {
      if (this.nowPage + 1 > this.pageCount) {
        this.nowPage = this.pageCount
      } else {
        this.nowPage++
      }
      this.$emit('next-click', this.nowPage)
    }
  }
}
</script>

<style>
  li{
    list-style:none
  }
  .active {
    pointer-events: none;
    filter: alpha(opacity=50);
    /*IE滤镜，透明度50%*/
    -moz-opacity: 0.5;
    /*Firefox私有，透明度50%*/
    opacity: 0.5;
    /*其他，透明度50%*/
  }

  .ul {
    text-align: center;
  }

  .lightNowPage {
    background-color: #409eff !important;
    color: #fff !important
  }

  .number {
    color: #606266;
    background-color: #f4f4f5;
    border-radius: 2px;
    min-width: 30px;
    float: left;
    vertical-align: top;
    line-height: 33px;
    height: 33px;
    margin: 0 5px;
    cursor: pointer;
  }
</style>
