<template>
  <!-- 遮罩 -->
  <transition name="fade-pupop">
    <div class="t-drawer-pupop" v-if="modelValue" @click="closeOfModal($event)"></div>
  </transition>
  <!-- 只要内容 -->
  <transition :name='animateName'>
    <div class="t-drawer" v-if="modelValue" :style="styles">
      <div class="t-drawer-contain-box">
        <div class="t-drawer-contain-box-header" :style="{'padding':showClose?'0 20px 0 45px':'0 20px 0 20px'}" v-if="showHeader">
          <slot name='header' v-if="$slot['header']"></slot>
          <i class="t-icon-close" v-if="showClose" @click="close"></i>
          <span v-if="!$slot['header']">{{title}}</span>
          <div class="t-drawer-contain-box-header-btn" v-if="!$slot['header']">
            <tButton size='small' :style="{'margin-right':confirmShow?'10px':'0'}" v-if="cancelShow" @click="cancelBtn">{{cancelText}}</tButton>
            <tButton size='small' type='primary' v-if="confirmShow" @click="confirmBtn">{{confirmText}}</tButton>
          </div>
        </div>
        <div class="t-drawer-contain-box-content" :style="{'height':showHeader?(showFooter?'calc(100% - 110px)':'calc(100% - 55px)'):showFooter?'calc(100% - 55px)':'100%'}">
          <slot name='content'></slot>
        </div>
        <div class="t-drawer-contain-box-footer" v-if="showFooter">
          <slot name="footer" v-if="$slot['footer']"></slot>
          <span v-else>生命中一个重要的过客，之所以是过客，因为你未曾为我停留!</span>
        </div>
      </div>
    </div>
  </transition>
</template>

<script>
export default {
  name:'tDrawer'
}
</script>
<script setup>
import { computed, useSlots } from 'vue'
const $slot=useSlots()
const emit=defineEmits(['update:modelValue','close','cancel','confirm'])
const props=defineProps({
  modelValue:Boolean,
  title:{
    type:String,
    default:'标题'
  },
  width:{
    type:String,
    default:"30%"
  },
  height:{
    type:String,
    default:"40%"
  },
  direction:{
    type:String,
    default:'left'
  },
  showHeader:{
    type:Boolean,
    default:true
  },
  showFooter:{
    type:Boolean,
    default:true
  },
  showClose:{
    type:Boolean,
    default:true
  },
  confirmText:{
    type:String,
    default:"确定"
  },
  cancelText:{
    type:String,
    default:"取消"
  },
  confirmShow:{
    type:Boolean,
    default:true
  },
  cancelShow:{
    type:Boolean,
    default:true
  },
  closeOnModal:{
    type:Boolean,
    default:true
  },
})

const styles=computed(()=>{
  return [
    props.direction=='left'?{"top":0,"left":0,"width":props.width,"height":"100%"}:
    props.direction=='top'?{"top":0,"left":0,"width":"100%","height":props.height}:
    props.direction=='right'?{"top":0,"right":0,"width":props.width,"height":"100%"}:
    props.direction=='bottom'?{"bottom":0,"left":0,"width":"100%","height":props.height}:{}
  ]
})

const animateName=computed(()=>{
  return props.direction=="left"?"slide-left":
         props.direction=="top"?"slide-top":
         props.direction=="right"?"slide-right":
         props.direction=="bottom"?"slide-bottom":"" 
})

const closeOfModal=(e)=>{
  if(props.closeOnModal){
    if(e.target.className=='t-drawer-pupop'){
      emit('update:modelValue',false)
      emit('close')
    }
  }
}

const close=()=>{
  emit('update:modelValue',false)
  emit('close')
}

const cancelBtn=()=>{
  emit('cancel')
}

const confirmBtn=()=>{
  emit('confirm')
}
</script>
<style lang="scss" scoped>
  .fade-pupop-enter-active,.fade-pupop-leave-active{
    transition: opacity 0.2s ease;
  }
  .fade-pupop-enter-from,.fade-pupop-leave-to{
    opacity: 0;
    transition: opacity 0.5s ease;
  }

  .slide-left-enter-active,.slide-left-leave-active{
    transition: all 0.3s ease;
    transform: translateX(0)
  }
  .slide-left-enter-from,.slide-left-leave-to{
    transform: translateX(-100%);
  }

  .slide-top-enter-active,.slide-top-leave-active{
    transition: all 0.3s ease;
    transform: translateY(0)
  }
  .slide-top-enter-from,.slide-top-leave-to{
    transform: translateY(-100%);
  }

  .slide-right-enter-active,.slide-right-leave-active{
    transition: all 0.3s ease;
    transform: translateX(0)
  }
  .slide-right-enter-from,.slide-right-leave-to{
    transform: translateX(100%);
  }

  .slide-bottom-enter-active,.slide-bottom-leave-active{
    transition: all 0.3s ease;
    transform: translateY(0)
  }
  .slide-bottom-enter-from,.slide-bottom-leave-to{
    transform: translateY(100%);
  }

  .t-drawer-pupop{
    width: 100%;
    height: 100%;
    position: fixed;
    top: 0;
    left: 0;
    background: rgba(0,0,0,.5);
    z-index: 8888;
  }
  .t-drawer{
    width: 30%;
    height: 100%;
    position: fixed;
    z-index: 8889;
    background: #fff;
    box-shadow: 0 2px 12px 0 rgba(7,7,7,0.1);
    .t-drawer-contain-box{
      width: 100%;
      height: 100%;
      overflow: hidden;
      .t-drawer-contain-box-header{
        width: 100%;
        height: 55px;
        display: flex;
        padding: 0 20px 0 45px;
        box-sizing: border-box;
        border-bottom: 1px solid #f0f0f0;
        align-items: center;
        font-size: 16px;
        color: #505050;
        position: relative;
        i{
          margin-right: 10px;
          position: absolute;
          top: 20px;
          left: 20px;
          cursor: pointer;
          font-size: 17px;
        }
        span{
          max-width: 70%;
          white-space: nowrap;
          overflow: hidden;
          text-overflow: ellipsis;
        }
        .t-drawer-contain-box-header-btn{
          position: absolute;
          right:20px;
        }
      }
      .t-drawer-contain-box-content{
        width: 100%;
        height: calc(100% - 110px);
        overflow-y: auto;
        padding: 20px;
        box-sizing: border-box;
        color: #505050;
        font-size: 14px;
      }
      .t-drawer-contain-box-footer{
        width: 100%;
        height: 55px;
        display: flex;
        padding: 0 20px;
        box-sizing: border-box;
        align-items: center;
        border-top: 1px solid #f0f0f0;
        color: #505050;
        font-size: 14px;
      }
    }
  }
  .t-drawer-contain-box-content::-webkit-scrollbar{
    width: 5px;
    height: 1px;
  }
  .t-drawer-contain-box-content::-webkit-scrollbar-thumb{
    border-radius: 4px;
    background: #adadad;
  }
  .t-drawer-contain-box-content::-webkit-scrollbar-track{
    border-radius: 0px;
    background: rgba(221,221,221);
  }
</style>


