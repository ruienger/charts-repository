<template>
  <div style="width:467px;height:calc(100% - 93px);display:flex;justify-content:space-between;position: relative;">
    <div class="round-chart-selecter">
      <div
        v-for="item in data"
        :key="item.name"
        @click="tagsChanged(item)"
        :style="item.shown?'color: #85c6ed':'color:#666'"
      >
        <div
          class="tiny-rect-center"
          :style="item.shown?'background-color: #10b9f5':'background-color:#333'"
        ></div>
        <span>{{ item.name }}</span>
        <span>{{ Math.floor(item.value/sum*100) }}%</span>
      </div>
    </div>
    <div class="round-chart-halo"></div>
    <div
      id="outPutStruct"
      style="width:269px;height:100%"
    >
    </div>

  </div>
</template>

<script>
const echarts = require("echarts");

export default {
  name: "outPutStruct",
  props: {
    // 数据结构
    // {
    //   '分类1'  e.g. '二氧化碳排放':
    //   [
    //     {
    //       name: '二级分类1' e.g. '小汽车',
    //       value: '值' e.g. 123
    //     },
    //     {
    //       name: '二级分类2' e.g. '客车',
    //       value: '值' e.g. 321
    //     }
    //   ],

    //   '分类2'  e.g. '燃油使用量':
    //   [
    //     {
    //       name: '二级分类1' e.g. '小汽车',
    //       value: '值' e.g. 123
    //     },
    //     {
    //       name: '二级分类2' e.g. '客车',
    //       value: '值' e.g. 321
    //     }
    //   ]
    // }
    data: {
      required: true,
      type: Array,
    },
  },
  data() {
    return {
      myChart: {},
      sum: 0,
      options: {
        title: {
          text: "排放结构",
          top: "middle",
          left: "center",
        },
        tooltip: {
          trigger: "item",
          formatter: "{a} <br/>{b}: {c} ({d}%)",
          backgroundColor: "#060D23",
          textStyle: {
            fontSize: 14,
            fontWeight: 400,
            color: "#fff",
          },
        },
        backgroundColor: "transparent",
        legend: {
          show: false,
          orient: "vertical",
          left: 10,
          top: "middle",
          width: 198,
          // height: 34,
          itemWidth: 13,
          itemHeight: 13,
          textStyle: {
            fontSize: 18,
            fontWeight: 400,
            color: "#85C6ED",
          },
          formatter: function (name) {
            return "<p>" + name + "</p>";
          },
          // borderWidth: 2,
          // borderColor: "#85C6ED",
          // shadowBlur: 10,
          // shadowColor: "lightbule",
          // shadowOffsetX: -5,
          // shadowOffsetY: -10,
          icon: "rect",
          data: ["货车", "出租", "公交", "大客车", "小客车"],
        },
        series: [
          {
            name: "访问来源",
            type: "pie",
            radius: ["60%", "70%"],
            avoidLabelOverlap: false,
            label: {
              show: false,
              position: "center",
            },
            data: this.data,
          },
        ],
      },
    };
  },
  watch: {
    data: {
      deep: true,
      handler() {
        this.data.forEach((e) => {
          e.shown = true;
        });
        this.refreshChart();
      },
    },
  },
  methods: {
    insertData(d) {
      let arr = [];
      let sum = 0;
      d.forEach((e) => {
        if (e.shown) sum += e.value;
      });
      d.forEach((e) => {
        if (e.shown) {
          arr.push(e);
          arr.push({
            value: (sum / 100) * 2,
            name: "",
            shown: true,
            itemStyle: {
              color: "#322112",
            },
            tooltip: {
              show: true,
            },
          });
        }
      });
      return arr;
    },
    refreshChart() {
      this.sum = 0;
      this.data.forEach((e) => {
        this.sum += e.value;
      });
      this.options.series[0].data = this.insertData(this.data);
      this.myChart.setOption(this.options);
    },
    tagsChanged(i) {
      i.shown = !i.shown;
      this.refreshChart();
    },
  },
  mounted() {
    //bind element for charts
    this.myChart = echarts.init(
      document.getElementById("outPutStruct"),
      "dark"
    );
    this.data.forEach((e) => {
      e.shown = true;
    });
    this.refreshChart(this.data);
  },
};
</script>

<style>
.round-chart-selecter {
  font-size: 18px;
  font-family: Source Han Sans SC;
  font-weight: 400;
  color: #85c6ed;
  display: flex;
  flex-direction: column;
  justify-content: space-around;
}
.round-chart-selecter span {
  display: inline-block;
  width: 40%;
}
.round-chart-selecter > div {
  width: 198px;
  height: 34px;
  line-height: 34px;
  border: 1px solid #406a8d;
  box-shadow: inset -1px -1px 5px #58d9f9;
  transition: all 0.3s;
}
.round-chart-selecter > div:hover {
  background-color: #58d9f933;
  box-shadow: inset -2px -2px 10px #58d9f9;
}
.tiny-rect-center {
  height: 14px;
  width: 14px;
  position: relative;
  top: 0;
  left: 0;
  right: 0;
  bottom: 0;
  margin: auto;
  display: inline-block;
  margin: 0 10px;
}
.round-chart-halo {
  height: 128px;
  width: 128px;
  border-radius: 64px;
  border: 2px solid #6397d6;
  position: absolute;
  top: 84px;
  left: 268px;
  box-shadow: 0 0 15px #6397d6;
  background-image: radial-gradient(#0c3260, #0c326022);
}
</style>