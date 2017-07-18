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
import VuePageSwipeList from './page-swipe-list.vue';
import VuePageSwipeItem from './page-swipe-item.vue';

export default {
  name: 'page-swipe',
  components: {
    'page-swipe-list': VuePageSwipeList,
    'page-swipe-item': VuePageSwipeItem
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
    let items = this.$slots['page-swipe-item'],
        length = items.length;
    return createElement(VuePageSwipeList, {
      props: {
        pages: items
      }
    }, items.map((item, $index) => {
      return createElement(VuePageSwipeItem, {
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
