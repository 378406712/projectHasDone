### vue使用alloyfinger做图片拖拽，旋转，缩放（移动端）

官网例子 ：http://alloyteam.github.io/AlloyFinger/

+ ```vue
  # npm install alloyfinger
  
  在页面 export default下:
  # import AlloyFinger from 'alloyfinger'
  
  mounted下:
  new AlloyFinger(element,{
  touchStart: function () { },
      touchMove: function () { },
      touchEnd:  function () { },
      touchCancel: function () { },
      multipointStart: function () { }, // 多点开始
      multipointEnd: function () { }, // 多点结束
      tap: function () { }, // 单点
      doubleTap: function () { }, // 双击
      longTap: function () { }, // 长按
      singleTap: function () { }, // 单击
      rotate: function (evt) { // 旋转
          console.log(evt.angle);
      },
      pinch: function (evt) { // 放大缩小
          console.log(evt.zoom);
      },
      pressMove: function (evt) { // 单点移动
          console.log(evt.deltaX);
          console.log(evt.deltaY);
      },
      swipe: function (evt) { // 滑动
          console.log("swipe" + evt.direction);
      }
  })
  
  
  ```

  ### example

  ```vue
  <template>
  <div
    id="jewelry"
    ref="jewelry"
    :style="{ left: `${left}`,
              top: `${top}`,
              transform: `rotateZ(${rotateZ}deg) scaleX(${scaleX}) scaleY(${scaleY})`
            }">
   <img ref="bigJewelry"
        :src="require(`@/assets/image/decorate/decorate-${jewelryType}-big.png`)"/>
  </div>
  </template>
   
  export default{
   data() {
      return {
        boxHeight: '', // 大盒子高度
        boxWidth: '', // 大盒子宽度
        diffX: '', // 鼠标点击物体那一刻相对于物体左侧边框的距离
        diffY: '',
        left: '35%', // 首饰位置
        top: '50%',
        rotateZ: 1,
        scaleX: 1,
        scaleY: 1,
      }
    },
    mounted() {
      const that = this
      this.boxWidth = this.$refs.photoContanier.clientWidth // 大盒子宽度
      this.boxHeight = this.$refs.photoContanier.clientHeight // 大盒子高度
  
      let jewelry = document.getElementById('jewelry') // 使用手势的元素节点
    	const drag = that.$refs.jewelry
      let initScale = 1 // 初始缩放倍率
      new AlloyFinger(jewelry, {
        rotate: function (evt) {
          that.rotateZ += evt.angle // 旋转
        },
        multipointStart() {
          initScale = that.scaleX
        },
        touchStart: function (e) {
          that.diffX = e.changedTouches[0].clientX - drag.offsetLeft //鼠标点击物体那一刻相对于物体左侧边框的距离=点击时的位置相对于浏览器最左边的距离-物体左边框相对于浏览器最左边的距离
          that.diffY = e.changedTouches[0].clientY - drag.offsetTop
        },
        pinch: function (evt) {
          that.scaleX = that.scaleY = initScale * evt.zoom
        },
        pressMove: function (e) {
          e.preventDefault() // 阻止图片默认拖动
          let left = e.changedTouches[0].clientX - that.diffX
          let top = e.changedTouches[0].clientY - that.diffY
          that.left = `${left}px`
          that.top = `${top}px`
          if (left < 0) {  // 拖动超过大盒子左边边界值，则left=0
            that.left = 0
          } else if (left > that.boxWidth - drag.offsetWidth) {
            that.left = `${that.boxWidth - drag.offsetWidth}px`
          }
          if (top < 0) {
            that.top = 0
          } else if (top > that.boxHeight - drag.offsetHeight) {
            that.top = `${that.boxHeight - drag.offsetHeight}px`
          }
          e.preventDefault()
        }
      })
    }
  }
  ```

