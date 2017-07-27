<!-- <template>
  <div id="app">
    <page-swipe-list @changeIndex="changeIndex" :pages="pages">
      <page-swipe-item v-for="(page, $index) in pages"
                       slot="item"
                       :key="$index"
                       :class="'item-' + $index"
                       :cur-idx="currentIndex">
        <div>Page</div>
      </page-swipe-item>
    </page-swipe-list>
  </div>
</template> -->

<script>
import PageSwipeList from './page-swipe-list.vue';
import PageSwipeItem from './page-swipe-item.vue';

export default {
  name: 'page-swipe',
  components: {
    PageSwipeList,
    PageSwipeItem
  },
  data() {
    return {
      currentIndex: 0
    }
  },
  props: {
    showIndicators: {
      type: Boolean,
      default: true
    },
    allowLoop: {
      type: Boolean,
      default: false
    },
    direction: {
      type: String,
      default: 'vertical'
    }
  },
  methods: {
    changeIndex(idx) {
      this.currentIndex = idx;
    }
  },
  render(createElement, context) {
    let direction = this.direction;
    if (direction != 'vertical' && direction != 'horizontal') {
      console.warn("[VuePageSwipe]: direction is not valid, it should be \"vertical\" or \"horizontal\"");
      direction = 'vertical';
    }

    let items = this.$slots.default;
    if (!items) {
      console.warn("[VuePageSwipe]: No tag found under <page-swipe>");
      return;
    }
    items = items.filter( item => {
      return item.tag != undefined;
    });
    let length = items.length;
    return createElement(PageSwipeList, {
      props: {
        pages: items,
        showIndicators: this.showIndicators,
        allowLoop: this.allowLoop
      },
      on: {
        changeIndex: this.changeIndex
      },
      'class': 'direction-' + direction
    }, items.map((item, $index) => {
      return createElement(PageSwipeItem, {
        props: {
          curIdx: this.currentIndex,
          index: $index,
          zIndex: length - $index
        },
        'class': 'item-' + $index,
        key: $index,
        slot: 'item'
      }, [item]);
    }));
  }
}
</script>
