<template>
  <div v-if="allData" class="virtual-list-wrap">
    <FixedHeightScroller
      :allData="allData"
      :virtualData.sync="virtualData"
      :itemHeight="60"
    >
      <template>
        <div v-for="(item, index) in virtualData" :key="index" class="item">
          {{ item }}
        </div>
      </template>
    </FixedHeightScroller>
  </div>
</template>

<script>
import FixedHeightScroller from "./FixedHeightScroller.vue";

export default {
  name: "FixedHeight",
  components: {
    FixedHeightScroller,
  },
  props: {
    allData: {
      type: Array,
      require: true,
    },
  },
  data() {
    return {
      virtualData: [],
    };
  },
  created() {},
  methods: {},
};
</script>

<style>
.virtual-list-wrap {
  height: 100%;
  background-color: beige;
}

.item {
  border-bottom: 1px solid #666;
  padding: 20px;
  height: 60px;
  text-align: center;
}
</style>
