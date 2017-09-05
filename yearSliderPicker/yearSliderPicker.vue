
<template>
  <div class="container">
      <div class="wrapper" ref="wrapper"  >
        <ul class="content" ref="wrapperul"  v-bind:style="{width:ulWidth + 'px'}">
          <li v-bind:id="'li_' +index" @click="selectItem(index)" ref="wrapperli" v-show="index!=0" v-for="(item,index) in years" v-bind:class="{red: index===middleItem}" v-bind:style="{width:liStyle + 'px'}">
            {{item}}
            <div  class="bottom-point">
              <a  class="round">●</a>
              </div>
              </li>
        </ul>
      </div>
      <!------------end-------------->
      <!--<button @click="clearInterval()" style="margin-top:20px">clearInterval</button>-->

  </div>

</template>

<script>

  import BScroll from 'better-scroll'

export default {
  name: 'test',
  data () {
    return {
        years: ['',1, 2, 3, 4, 5, 6, 7, 8, 9, 10, 11, 12, 13, 14, 15, 16, 17, 18, 19, 20 , 21, 22, 23, 24 ,25 , 26 ,27 , 28, 29, 30],
        middleItem: '',
        width: document.documentElement.clientWidth , //屏幕宽度 （没用到。。。）
        liStyle:  document.documentElement.clientWidth / 9 ,  //li标签宽度
        ulWidth: document.documentElement.clientWidth / 9 * 30 + 10, //ul 标签宽度  放下 30 个点
        scrollflage: false
      
    }
  },
mounted () {
      this.$nextTick(() => {
        this.scroll = new BScroll(this.$refs.wrapper, {
          click: true, 
          startX: 0, //x起始位置
          startY: 0,//y起始位置
          scrollX: true, //只允许 x方向滑动
          bounce: true, //开启回弹
          probeType : 3, //scroll 控制
          swipeTime: 5 //边缘回弹时间

        })
        this.scroll.scrollToElement(document.querySelector('#li_22'), 100)//滑动到22  
        
        this.middleItem = 30//定位到30 变大

        this.scroll.on('scroll', (pos) => { //滑动触发
            if(this.scrollflage == true){//解决首次定位到30的问题
                this.middleItem = Math.round(Math.abs(pos.x / this.liStyle) ) + 5 //判断中间元素
            }
          })
        this.scroll.on('scrollStart', (pos) => { //再滑动开始的时候放开滑动判断
              this.scrollflage = true //开启滑动判断
          })
       
      })
     
     

    },
  methods: {
    selectItem: function(e){ //click 手动选择时间
      this.middleItem = e
    },
  },
      



}
</script>

<style scoped>
 .container {
    background: -moz-linear-gradient(left, #3FC2F1  0%, #3479DE 100%);
    background: -webkit-linear-gradient(left, #3FC2F1  0%, #3479DE 100%);
    background: linear-gradient(to right, #3FC2F1 0%,#3479DE 100%);
  }
  .wrapper{
    height:100px;
    width:100%;
    overflow:hidden;
    margin-top:20px;
    background-color:white;
  }
  ul{
    height:100px;
    padding-left:0px; 
  }
  li{
    text-align:center;
    height:20px;
    line-height:20px;
    float:left;
    padding-top: 30px;
    list-style-type:none;
  }
  .red{
    color:red;
    font-size: 35px;
    padding-top: 20px;
  }
  .round{width:16px;height:16px;display: inline-block;font-size:20px;line-heigth:16px;text-align:center;color:#f00;text-decoration:none}
  .red .round{
    font-size: 35px;
     padding-top: 8px;
  }

</style>
