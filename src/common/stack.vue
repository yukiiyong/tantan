<template>
	<ul class="stack">
		<li class="stack-item" v-for="(item, index) in pages" :style="[transform(index),transformIndex(index)]" @touchstart.stop.prevent="touchstart" @touchmove.stop.prevent="touchmove" @touchend.stop.prevent="touchend" @mousedown.stop.prevent="touchstart" @mousemove.stop.prevent="touchmove" @mouseup.stop.prevent="touchend">
      <img :src='item.src' :alt='item.alt'>
    </li>
  </ul>
</template>
<script type="text/ecmascript-6">

export default {
  props: {
    pages: {
      type: Array,
      default: () => []
    }
  },
  data() {
    return {
      basicData:{
        //记录首页索引
        currentPage: 0
      },
      temporyData: {
        //首页
        opacity: 1,
        zIndex: '',
        visible: 3
      },
      touch:{
        initiated: false,
        deltaX: 0,
        deltaY: 0,
        startX: 0,
        startY: 0
      }
    }
  },
  methods: {
    touchstart(e) {
      if(this.touch.initiated) {
        return 
      }
      this.touch.initiated = true
      if(e.type === 'touchstart') {
        if(e.touches.length > 1) {
          this.touch.initiated = false
          return 
        } else {
          const touch = e.touches[0]
          this.touch.startX = touch.pageX
          this.touch.startY = touch.pageY
        }
      } else {
        this.touch.startX = e.clientX
        this.touch.startY = e.clientY
      }
      
    },
    touchmove(e) {
      if(!this.touch.initiated) {
        return 
      }
      if(e.type === 'touchmove') {
        const touch = e.touches[0]
        this.touch.deltaX = touch.pageX-this.touch.startX
        this.touch.deltaY =  touch.pageY - this.touch.startY
        if(Math.abs(this.touch.deltaX) < Math.abs(this.touch.deltaX)) {
          return 
        }
      } else {
        this.touch.deltaX = e.clientX - this.touch.startX
        this.touch.deltaY = e.clientY - this.touch.startY
      }
      
    },
    touchend(e) {
      if(!this.touch.initiated) {
        return 
      }
      this.touch.moved = true
      if(Math.abs(this.touch.deltaX) > 100) {
        this.touch.deltaX = this.touch.deltaX > 0 ? this.touch.deltaX + 200 : this.touch.deltaX - 200
        this.touch.deltaY = this.touch.deltaY > 0 ? this.touch.deltaY + 200 : this.touch.deltaY - 200
        this.temporyData.opacity = 0
        this.basicData.currentPage = this.basicData.currentPage === this.pages.length - 1 ? 0 : this.basicData.currentPage + 1
        this.$nextTick(() => {
          this.touch.deltaX = 0
          this.touch.deltaY = 0
          this.temporyData.opacity = 1
        })
      } else {
        this.touch.deltaX = 0
        this.touch.deltaY = 0 
      }
      this.touch.initiated = false
    },
    transform(index) {
      let style = {}
      let currentPage = this.basicData.currentPage
      let perIndex = index - currentPage
      if(index === this.basicData.currentPage) {
        return 
      }
      if((index > currentPage) && (index - this.basicData.currentPage) < this.temporyData.visible) {
        style['transform'] = `translate3d(0, 0, ${-60 * perIndex}px)`
        style['opacity'] = 1 
        style['zIndex'] = this.temporyData.visible - index + this.basicData.currentPage
      } else {
        style['transform'] = `translate3d(0, 0, ${-60 * this.temporyData.visible}px)`
        style['zIndex'] = -1
        style['opacity'] = 0
      }
      return style
    },
    transformIndex(index) {
      if(index === this.basicData.currentPage) {
        let style = {}
        style['transform'] = `translate3d(${this.touch.deltaX}px, ${this.touch.deltaY}px, 0)`
        style['opacity'] = this.temporyData.opacity
        style['zIndex'] = 10
        if(this.touch.moved) {
          style['transition'+ 'TimingFunction'] = 'ease'
          style['transition'+'Duration'] = 300 + 'ms' 
        }
        return style 
      }
    } 
  }
}
</script>

<style rel="stylesheet/stylus" lang="stylus" scoped>
.stack  
  position:relative
  width: 100%
  height: 100%
  perspective: 1000px
  perspective-origin: 50% 150%
  -webkit-perspective: 1000px
  -webkit-perspective-origin: 50% 150%
  margin: 0 
  padding: 0
  .stack-item
    width: 100%
    height: 100%
    border-radius: 4px
    text-align: center
    overflow: hidden
    position: absolute
    display: flex
    flex-direction: column
    -webkit-flex-direction: column
    user-select: none
    -webkit-user-select: none
    -moz-user-select: none
    -ms-user-select: none
    pointer-events: auto
    img
      width: 100%
      display:block
      pointer-events: none
</style>