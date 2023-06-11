<template>
  <div class="virtual-list-container">
    <div
      ref="virtualList"
      class="virtual-list-visible-container"
      @scroll="scrollEvent($event)"
    >
      <div
        class="virtual-list-all-container"
        :style="{ height: allHeight + 'px' }"
      >
        <div class="virtual-list" :style="{ transform: getTransform }">
          <slot :virtualData="inVirtualData"></slot>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  name: "FixedHeightScroller",
  props: {
    // 全部数据
    allData: {
      type: Array,
      default: () => [],
    },
    // 虚拟数据
    virtualData: {
      type: Array,
      default: () => [],
    },
    itemHeight: {
      // 每项高度
      type: [Number, String],
      default: "60",
    },
  },
  data() {
    return {
      startIndex: 0,
      endIndex: 0,
      offset: 0, // 偏移量
      inVirtualData: [],
    };
  },
  computed: {
    allHeight() {
      return this.allData.length * Number(this.itemHeight);
    },
    //可见数量
    visibleCount() {
      return Math.ceil(this.showHeight / Number(this.itemHeight));
    },
    // 显示数据的偏移量
    getTransform() {
      return `translate(0,${this.offset}px)`;
    },
  },
  watch: {
    allData: {
      handler() {
        this.inVirtualData = this.allData.slice(this.startIndex, this.endIndex);
        
        this.$emit("update:virtualData", this.inVirtualData);
      },
      deep: true,
    },
  },
  mounted() {
    this.showHeight = this.$el.clientHeight;
    this.startIndex = 0;
    this.endIndex = this.startIndex + this.visibleCount;
    this.inVirtualData = this.allData.slice(this.startIndex, this.endIndex);
    this.$emit("update:virtualData", this.inVirtualData);
  },
  methods: {
    scrollEvent(e) {
      const scrollTop = e.target.scrollTop;
      this.startIndex = Math.floor(scrollTop / this.itemHeight);
      this.endIndex = this.startIndex + this.visibleCount;
      //偏移量 = 滚动距离
      this.offset = scrollTop - (scrollTop % this.itemHeight);
      this.inVirtualData = this.allData.slice(this.startIndex, this.endIndex);
      this.$emit("update:virtualData", this.inVirtualData);
    },
  },
};
</script>
<style scoped>
.virtual-list-container {
  height: 100%;
}

.virtual-list-visible-container {
  position: relative;
  /*  */
  overflow: auto;
  height: 100%;
}

/* 全部高度 撑出滚动条 */
.virtual-list-all-container {
  position: absolute;
  left: 0;
  top: 0;
  right: 0;
  height: auto;
}

/* 显示数据 */
.virtual-list {
  position: absolute;
  left: 0;
  top: 0;
  right: 0;
  height: auto;
}
</style>
