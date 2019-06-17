<template>
  <div class="home-parent-box">
    <div class="home-header" :style="'height:'+header_height+'px;'">
      <button type="button" @click="expend_body_left">expand</button>
    </div>
    <div class="home-body" :style="'height:'+bodyHeight+'px;'">
        <div :class="body_left+' home-body-left'" :style="'height:'+bodyHeight+'px;'">
          <slot name="left_box" ></slot>
        </div>
        <div class="home-body-right" :style="'width:'+bodyRightWidth+'px;height:'+bodyHeight+'px;'">
          <slot name="right_box"></slot>
        </div>
        <div style="clear:both;"></div>
    </div>
  </div>
</template>

<script>
import { setInterval,setTimeout } from 'timers';
export default {
  name: 'home',
  props:["header_height"],
  data(){
    return {
      screenHeight:0,
      screenWidth:0,
      bodyRightWidth:0,
      body_left:"fixed-width",
      isExpand:true,
      leftBoxWidth:0,
      leftDom:null
    }
  },
  computed:{
    bodyHeight(){
      return this.screenHeight-this.header_height;
    }
  },
  methods:{
    expend_body_left(){
      this.isExpand = !this.isExpand;
      this.$emit("trigger_toggle_button",this.isExpand)
    }
  },
  mounted () {
    const that = this
    //设置初始值
    window.screenHeight = document.body.clientHeight
    window.screenWidth = document.body.clientWidth
    that.screenHeight = window.screenHeight
    that.screenWidth = window.screenWidth
    if(that.screenWidth>768){
      if(that.isExpand){
        that.body_left = "fixed-width";
      }else{
        that.body_left = "auto-width";
      }
      that.bodyRightWidth = document.body.clientWidth-document.getElementsByClassName("home-body-left")[0].offsetWidth;
    }else{
      if(that.isExpand){
        that.body_left = "hidden-self";
      }else{
        that.body_left = "auto-width";
      }
      that.bodyRightWidth = that.screenWidth
    }
    //监听浏览器大小变化后重设相关值，为了让值不频繁的变化导致卡顿，进行特殊处理
    let isReady = true;
    window.onresize = () => {
      return (() => {
        setTimeout(function(){
          if(!isReady){
            return;
          }
          window.screenHeight = document.body.clientHeight
          window.screenWidth = document.body.clientWidth
          that.screenHeight = window.screenHeight
          that.screenWidth = window.screenWidth
          if(that.screenWidth>768){
            if(that.isExpand){
              that.body_left = "fixed-width";
            }else{
              that.body_left = "auto-width";
            }
            that.bodyRightWidth = document.body.clientWidth-document.getElementsByClassName("home-body-left")[0].offsetWidth;
          }else{
            if(that.isExpand){
              that.body_left = "hidden-self";
            }else{
              that.body_left = "auto-width";
            }
            that.bodyRightWidth = that.screenWidth
          }
        },20);
        isReady = false;
        setTimeout(function(){
          isReady = true;
        },10);
      })()
    }
    //轮询监听dom的变化
    setInterval(function(){
      if(that.screenWidth<768){
        return;
      }
      that.bodyRightWidth = that.screenWidth-document.getElementsByClassName("home-body-left")[0].offsetWidth;
      that.$nextTick(function(){
        that.bodyRightWidth = that.screenWidth-document.getElementsByClassName("home-body-left")[0].offsetWidth;
      })
    },10);
  },
  watch: {
    isExpand(val){
      //点击伸缩菜单时，左侧div切换不同的样式，并且右侧的div响应变化
      if(val){
        if(this.screenWidth>768){
          this.body_left = "fixed-width";
        }else{
          this.body_left = "hidden-self";
        }
      }else{
        this.body_left = "auto-width";
      }
      const that = this
      this.$nextTick(function(){
        if(that.screenWidth>768){
          that.bodyRightWidth = that.screenWidth-document.getElementsByClassName("home-body-left")[0].offsetWidth;
        }else{
          that.bodyRightWidth = that.screenWidth
        }
      })
    }
  }
}
</script>
<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
.home-parent-box{
  overflow: hidden;
}
.home-header{
  background: rgb(5, 160, 187);
}
.home-body{
  background: rgb(57, 57, 238);
}
.home-body-left{
  position: fixed;
  left: 0;
  bottom: 0;
  overflow: hidden;
  background: rgb(188, 229, 240);
  opacity:0.8;
}
.home-body-right{
  float: right;
  background: rgb(124, 221, 124);
}
.fixed-width{
  width: 50px;
}
.auto-width{
  min-width: 200px;
}
.hidden-self{
  display: none;
}
</style>
