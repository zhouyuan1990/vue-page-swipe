<template>
  <li class="page-swipe-item item-easing"
      :class="{ 'is-active': curIdx == index, 'moveOut': index < curIdx }"
      :style="{ zIndex: zIndex}">
    <slot></slot>
  </li>
</template>

<script>
export default {
  name: 'page-swipe-item',
  props: {
    curIdx: {
      type: Number,
      default: 0
    },
    index: {
      type: Number,
      default: -1
    },
    zIndex: {
      type: Number,
      default: 0
    }
  }
}
</script>

<style scoped>
.page-swipe-item {
  position: absolute;
  height: 100%;
  width: 100%;
  transform: translate3d(0px, 0px, 0px);
}
.page-swipe-item > div {
  width: 100%;
  height: 100%; 
}
.item-easing {
  transition: transform 1000ms cubic-bezier(0.550, 0.085, 0.000, 0.990);
}
.moveOut {
  transform: translate3d(0px, -100%, 0px);
}
</style>