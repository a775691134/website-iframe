<template>
  <div v-if="show" class="wrap" :class="{'bg-blur': isBlur}">
    <div class="timedown">{{continueTime}}</div>
    <img src="@/assets/close@3x.png" alt="" @click="close">
    <a @click="gotoHref(data.url)">
      <el-image :style="{height: height, width: width}" :src="data.img" :fit="fit"/>
    </a>
  </div>
</template>

<script setup>
import { ref } from 'vue'

const props = defineProps({
  data: {
    type: Object
  },
  fit: {
    type: String,
    default: "cover"
  },
  height: {
    type: String,
    default: "100%"
  },
  width: {
    type: String,
    default: "100%"
  },
  isBlur: {
    type: Boolean,
    default: false
  }
})

const show = ref(false)
const close = () => {
  show.value = false
}

const continueTime = ref(null)

const timerOpen = () => { 
  setTimeout(() => {
    show.value = true
    timerClose()
  }, props.data.startTime * 1000)
}

continueTime.value = props.data.continueTime
console.log(props.data.continueTime, 'props.data.continueTime')
const timerClose = () => { 
  if (continueTime.value <= 0) {
    close()
    return
  }
  setTimeout(() => {
    continueTime.value -= 1
    timerClose()
  }, 1000)
}

timerOpen()
</script>

<style lang="less" scoped>
  .bg-blur {
    backdrop-filter: blur(1px);
  }
  .wrap {
    position: relative;
    margin-bottom: -10px;
    height: 100%;
    img {
      position: absolute;
      top: 0;
      right: 0;
      width: 20px;
      height: 20px;
      object-fit: cover;
      z-index: 1;
      // background-color: #fff;
    }
    .timedown {
      position: absolute;
      top: 20px;
      right: 5px;
      color: #fff;
      font-size: 16px;
      object-fit: cover;
      z-index: 1;
    }
  }
</style>