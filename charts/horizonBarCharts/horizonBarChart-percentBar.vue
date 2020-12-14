<template>
  <div style="height: calc(100% - 93px);
  box-sizing: border-box;display:flex;flex-direction:column;justify-content:space-between">
    <div
      v-for="item in data"
      :key="item.name"
      class="hoz-bar"
    >
      [ {{ item.name }} ]
      <div>
        <div
          class="hoz-filled-bar"
          :style="'width: ' + item.value*100 + '%'"
        ></div>

      </div>
      <span class="hoz-bar-span">{{ Math.floor(item.value*100) }}%</span>
    </div>
  </div>
</template>

<script>
export default {
  name: "hozBarCharts",
  props: {
    // 数据格式
    // [
    //   {
    //     name: 名称1  e.g. tom,
    //     value: 百分比小数  e.g. 0.6,
    //   },
    //   ...
    // ]
    data: {
      required: true,
      type: Array,
    },
  },
};
</script>

<style>
.hoz-bar-span {
  font-size: 16px;
  font-weight: 400;
  color: #51e6ff;
}
.hoz-bar {
  color: #eee;
  padding: 5px 0;
}
.hoz-bar div {
  height: 14px;
}
.hoz-bar > div {
  width: 88%;
  background-color: #00000044;
  display: inline-block;
  margin-right: 15px;
  border: 2px solid #1c3b68;
}
.hoz-filled-bar {
  background-image: linear-gradient(90deg, #0d4590, #04abe1);
  transition: all 0.5s;
}
</style>