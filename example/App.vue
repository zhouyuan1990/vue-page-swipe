<template>
  <div id="app">
    <page-swipe>
      <div swipe-title="page1" class="page1">Page1</div>
      <div swipe-title="page2" class="page2">Page2</div>
      <div swipe-title="page3" class="page3">Page3</div>
    </page-swipe>
  </div>
</template>
<script>
import PageSwipe from '../dist/vue-page-swipe';

export default {
  name: 'app',
  components: {
    'page-swipe': PageSwipe
  }
}
</script>
<style scoped>
.page1 {
  background-color: rgb(238, 0, 90);
}
.page2 {
  background-color: rgb(44, 62, 80);
}
.page3 {
  background-color: rgb(51, 153, 204);
}
</style>