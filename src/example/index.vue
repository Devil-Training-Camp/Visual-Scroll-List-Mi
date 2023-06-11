<template>
  <div class="demo">
    <fixed-height :allData="data"></fixed-height>
  </div>
</template>

<script>
import FixedHeight from "@/components/FixedHeight.vue";
export default {
  components: { FixedHeight },
  data() {
    return {
      data: null,
      virtualData: [],
    };
  },
  created() {
    setTimeout(() => {
      this.getData();
    }, 1000);
  },
  methods: {
    getData() {
      const data = [];
      for (let i = 0; i <= 10000; i++) {
        data[i] = i;
      }
      this.data = data;
    },
  }
};
</script>

<style scoped>
.demo{
  height: 100%;
}
</style>
