<template>
  <v-chart
    :options="newOption"
    :auto-resize="true"
    @click="clickThis"
  ></v-chart>
</template>

<script>
export default {
  name: "barChart",
  props: {
    // 数据格式
    // [
    //     {
    //       name: '电站',
    //       data: 4
    //     },
    //     {
    //       name: '架空设备',
    //       data: 5
    //     },
    // ]
    // colorSet 用来配置颜色
    data: {
      type: Array,
      reqiured: true,
    },
    colorSet: {
      type: Array,
      default: () => {
        return [
          "#fffc43",
          "#43d9ff",
          "#ff4943",
          "#ca43ff",
          "#ff43c0",
          "#459456",
        ];
      },
    },
  },
  watch: {
    data: {
      deep: true,
      handler() {
        this.reFresh();
      },
    },
  },
  data() {
    return {
      newOption: {},
      option: {
        tooltip: {
          trigger: "item",
        },
        legend: {
          show: false,
        },
        series: [
          {
            type: "pie",
            top: "top",
            left: "center",
            itemStyle: {
              color: "#ff9f57",
            },
            data: [],
            roseType: "radius",
            radius: ["20%", "55%"],
            labelLine: {
              show: true,
              length2: 40,
              lineStyle: {
                color: "#45a579",
              },
            },
            label: {
              show: true,
              color: "#45a579",
              fontSize: 17,
              position: "outside",
              padding: [5, 0, 0, -50],
              formatter: (params) => {
                return params.dataIndex > 2
                  ? "{l|" + params.name + "}\n \n {lx|" + params.percent + "}"
                  : "" + params.name + "\n \n {rx|" + params.percent + "}";
              },
              rich: {
                l: {
                  fontSize: 17,
                  color: "#45a579",
                  padding: [5, -50, 0, 0],
                },
                rx: {
                  fontSize: 23,
                  color: "#ffdc2f",
                },
                lx: {
                  padding: [5, -50, 0, 0],
                  fontSize: 23,
                  color: "#ffdc2f",
                },
              },
            },
          },
        ],
      },
    };
  },
  methods: {
    reFresh() {
      this.option.legend.data = this.data.slice(0, 4).map((e) => {
        return e.name;
      });
      this.option.series[0].data = [];
      this.data.forEach((element, i) => {
        let color = this.colorSet[i];
        this.option.series[0].data.push({
          value: element.data,
          name: element.name,
          itemStyle: { color },
        });
      });
      this.newOption = this.option;
    },
    clickThis() {},
  },
  mounted() {
    this.option.backgroundColor = "transparent";
    this.reFresh();
  },
};
</script>

<style scoped>
</style>