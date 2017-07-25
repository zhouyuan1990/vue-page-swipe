# vue-page-swipe

a page swipe using vue.js (using slot and render)

# Using vue-page-swipe

## Install
```bash
$ npm install vue-page-swipe
```

## Import and register module
```JavaScript
import PageSwipe from 'vue-page-swipe';
Vue.component('page-swipe', PageSwipe);
```
## Using "page-swipe" tag simply
```HTML
<page-swipe>
  <div swipe-title="page1" class="page1">Page1</div>
  <div swipe-title="page2" class="page2">Page2</div>
  <div swipe-title="page3" class="page3">Page3</div>
</page-swipe>
```

# Develop

Run example:
```bash
$ npm run dev
```

Build src to /dist
```bash
$ npm run build
```

# Options
| Option | Description |
| ----- | ----- |
| showIndicators | Boolean (default: true) - Defines whether indicators are shown or not. |
| allowCycles | Boolean (default: false) - If set to true, scrolling up in the first page will scroll to the last page, and scrolling down in the last page will scroll to the first page.

# Demo
![img](https://raw.githubusercontent.com/zhouyuan1990/vue-page-swipe/master/demo.gif)
