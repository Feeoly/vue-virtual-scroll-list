<template>
  <div class="example">
    <github-corner />
    <introduction description="The size of each item is equal." />

    <div class="example-content">
      <tab v-on:tab-change="onTabChange" />

      <div v-show="isShowView">
        <virtual-list class="list scroll-touch"
          :data-key="'id'"
          :data-sources="items"
          :data-component="itemComponent"
          :estimate-size="100"
          :item-class="'list-item-fixed item'"
          :keep="30"
        />
      </div>
    </div>
  </div>
</template>

<script>
import Item from './Item'

import { Random } from '../../common/mock'
import genUniqueId from '../../common/gen-unique-id'
import { TAB_TYPE, DEFAULT_TAB } from '../../common/const'

const TOTAL_COUNT = 10000

const DataItems = []
let count = TOTAL_COUNT
while (count--) {
  const index = TOTAL_COUNT - count
  DataItems.push({
    index,
    name: Random.name(),
    id: genUniqueId(index),
  })
}

export default {
  name: 'fix-size',

  data () {
    return {
      total: TOTAL_COUNT.toLocaleString(),
      items: DataItems,
      itemComponent: Item,
      isShowView: DEFAULT_TAB === TAB_TYPE.VIEW,
      itemHeight: 0,
    }
  },

  methods: {
    recalculate() {
      var item = 
          this.$refs.templateItem ? 
          this.$refs.templateItem.offsetHeight : 
          document.querySelector('#app .item').offsetHeight
      
      this.itemHeight = 500 / item / 2
      console.log('item height:', this.itemHeight, item)
    },
    onTabChange (type) {
      this.isShowView = type === TAB_TYPE.VIEW
    }
  },
  mounted() {
    this.recalculate()
    window.addEventListener('resize', this.recalculate)
  }
}
</script>

<style lang="less">
.list {
  width: 100%;
  height: 500px;
  border: 2px solid;
  border-radius: 3px;
  overflow-y: auto;
  border-color: dimgray;

  .list-item-fixed {
    display: flex;
    align-items: center;
    padding: 0 1em;
    height: 60px;
    border-bottom: 1px solid;
    border-color: lightgray;
  }
  .list-item-fixed{
    border: 1px solid;
    width: 30%;
    height: 100px;
    float: left;
  }
  .item {
    background: lightseagreen;
    margin: 5px;
    cursor: pointer;
    min-width: 60px;
    flex-basis: calc(12.5% - 10px);
  }
  .item:nth-child(even) {
      background: blue;
  }
}
</style>
