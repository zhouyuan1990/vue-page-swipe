<template>
  <div class="page-swipe-list" tabindex="1"
       @mouseWheel.prevent="handleWheel"
       @wheel.prevent="handleWheel"
       @keydown="handleKeyDown">
    <ul class="main">
      <slot name="item"></slot>
    </ul>
    <ul class="indicators" ref="indicators" v-show="showIndicators">
      <li class="indicator"
          v-for="(page, $index) in pages"
          :class="{ 'is-active': $index === index }"
          :key="$index">
        <a @click="moveTo($index)"></a>
        <div v-if="page.data.attrs && page.data.attrs['swipe-title']">{{ page.data.attrs['swipe-title'] }}</div>
      </li>
    </ul>
  </div>
</template>

<script>
let preWheelTime = 0,
    lastAnimationTime = 0,
    wheels = [];

export default {
  name: 'page-swipe-list',
  data() {
    return {
      index: 0
    }
  },
  props: {
    pages: {
      type: Array,
      default: function() {
        return [];
      }
    },
    passIndex: {
      type: Function,
      default: function () {}
    },
    showIndicators: {
      type: Boolean,
      default: true
    },
    allowLoop: {
      type: Boolean,
      default: false
    }
  },
  methods: {
    isMoving() {
      let curTime = new Date().getTime();
      let animationDuration = 1000;
      if (curTime - lastAnimationTime < animationDuration + 200) {
        return true;
      }
      return false;
    },
    getWheelDelta(event) {
      let value = event.wheelDelta || -event.deltaY || -event.detail;
      let delta = Math.max(-1, Math.min(1, value));
      return delta;
    },
    isAccelerating() {
      if (wheels.length < 10) return true;
      let averageEnd = this.getLastWheelsAverage(10);
      let averageMiddle = this.getLastWheelsAverage(50);
      return averageEnd >= averageMiddle;
    },
    getLastWheelsAverage(number) {
      let sum = 0;
      let calcElements = wheels.slice(Math.max(wheels.length - number, 1));
      for (let i = 0; i < calcElements.length; i++) {
        sum += calcElements[i];
      }
      return Math.ceil(sum/number);
    },
    handleWheel(event) {
      let curWheelTime = new Date().getTime();
      let timeDiff = curWheelTime - preWheelTime;
      preWheelTime = curWheelTime;
      if (timeDiff > 200) {
        wheels = [];
      }
      if (wheels.length > 99) {
        wheels.shift();
      }
      let delta = this.getWheelDelta(event);
      wheels.push(Math.abs(delta));

      if (!this.isMoving() && this.isAccelerating()) {
        if (delta > 0) {
          this.prev();
        } else {
          this.next();
        }
      }
    },
    handleKeyDown(event) {
      if (this.isMoving()) return;

      switch(event.keyCode) {
        case 38:  //ArrowUp
        case 33:  //PageUp
        case 37:  //ArrowLeft
            this.prev();
            break;
        case 40:  //ArrowDown
        case 34:  //PageDown
        case 39:  //ArrowRight
            this.next()
            break;
        case 36:  //Home
            this.moveToFirst();
            break;
        case 35:  //End
            this.moveToLast();
            break;
        default:
            return;
      }
    },
    next() {
      if (this.index < this.pages.length - 1) {
        this.moveTo(this.index + 1);
      } else if (this.allowLoop) {
        this.moveToFirst();
      }
    },
    prev() {
      if (this.index > 0) {
        this.moveTo(this.index - 1);
      } else if (this.allowLoop) {
        this.moveToLast();
      }
    },
    moveToFirst() {
      this.moveTo(0);
    },
    moveToLast() {
      this.moveTo(this.pages.length - 1);
    },
    moveTo(index) {
      if (index != this.index) {
        lastAnimationTime = new Date().getTime();
        this.index = index;
        this.passIndex && this.passIndex(this.index);
      }
    }
  }
}
</script>
<style>
body {
  padding: 0;
  margin: 0;
  font-weight: 400;
}
ul, li {
  list-style: none;
  margin: 0;
  padding: 0;
}
div:focus {
  outline: none;
}
</style>

<style scoped>
.indicators {
  position: absolute;
  z-index: 100;
}
.main {
  position: absolute;
  height: 100%;
  width: 100%;
}
.indicators {
  top: 50%;
  transform: translateY(-50%);
  margin-left: 0.7rem;
}
.indicator {
  position: relative;
  height: 0.7rem;
  width: 0.7rem;
  padding: 0.15rem 0 0.15rem 0;
}
.indicator > a {
  position: relative;
  display: block;
  width: 100%;
  height: 100%;
  box-sizing: border-box;
  border-radius: 50%;
  background-color: #fff;
  opacity: 0.25;
  cursor: pointer;
}
.indicator.is-active > a {
  opacity: 0.9;
}
.indicator > div {
  position: absolute;
  white-space: nowrap;
  left: 1rem;
  top: 50%;
  transform: translateY(-50%);
  font-size: 0.8rem;
  color: #fff;
  transition: opacity 200ms linear;
}
.indicator > a + div {
  opacity: 0;
}
.indicator > a:hover + div {
  opacity: 1;
}
</style>
