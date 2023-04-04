<template>
  <div
    @scroll="handleScroll"
    style="height: 700px; width: 300px; overflow-y: auto"
  >
    <div
      v-for="(item, index) in list"
      :key="index"
      class="box"
      :style="{ background: item.background }"
    >
      {{ index }}
    </div>
    <div v-show="list.length<50">加载中...</div>
  </div>
</template>

<script>
import { reactive } from "vue";
export default {
  name: "myList",
  setup() {
    let list = reactive([{ background: "rgb(233,32,38)" }]);
    let timer = null;
    function getList(num) {
      for (let i = 0; i < num; i++) {
        let c1 = Math.floor(Math.random() * 256);
        let c2 = Math.floor(Math.random() * 256);
        let c3 = Math.floor(Math.random() * 256);
        list.push({ background: "rgb(" + c1 + "," + c2 + "," + c3 + ")" });
      }
    }
    getList(10);
    const handleScroll = (e) => {
      if (timer) return;
      timer = setTimeout(() => {
        timer = null;
        const scrollWrapper = e.target;
        const scrollTop = scrollWrapper.scrollTop;
        const wrapperHeight = scrollWrapper.clientHeight;
        const scrollHeight = scrollWrapper.scrollHeight;
        if (scrollTop + wrapperHeight >= scrollHeight / 2 && list.length < 50) {
          getList(10);
        }
      }, 2000);
    };
    return {
      list,
      handleScroll,
    };
  },
};
</script>
<style lang='less' scoped >
.box {
  width: 100%;
  height: 500px;
}
</style>