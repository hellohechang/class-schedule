<template>
  <router-view></router-view>
  <a href="https://github.com/hellohechang/class-schedule" target="_blank"><img src="./assets/github.png" alt=""></a>
</template>

<script setup>
import { onMounted } from 'vue';
onMounted(() => {
  let evt = 'onorientationchange' in window ? 'orientationchange' : 'resize';
  function computed() {
    let HTML = document.documentElement,
      devicedW = HTML.clientWidth,
      designW = 1000,
      ratio = devicedW / designW * 100;
    // devicedW >= designW ? ratio = 100 : null;
    HTML.style.fontSize = ratio + 'px'
  }
  computed()
  window.addEventListener(evt, computed);
})
</script>

<style lang="less" scoped>
a {
  position: fixed;
  bottom: 10px;
  right: 10px;

  img {
    width: 30px;
  }

}
</style>