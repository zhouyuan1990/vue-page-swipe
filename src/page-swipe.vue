<!-- <template>
  <div id="app">
    <page-swipe-list :pages="pages">
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
  methods: {
    passIndex(idx) {
      this.currentIndex = idx;
    }
  },
  render(createElement, context) {
    let items = this.$slots.default;
    if (!items) {
      console.warn("[VuePageSwipe]: No tag found under <page-swipe>");
      return;
    }
    // Filter out empty node in items, after set preserveWhitespace: false in vue-loader config, these lines are commented out
    // items = items.filter( item => {
    //   return item.tag != undefined;
    // });
    let length = items.length;
    return createElement(PageSwipeList, {
      props: {
        pages: items,
        passIndex: this.passIndex
      }
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
