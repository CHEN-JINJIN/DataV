<template>
  <div class="center_bottom">
    <Echart :options="options" id="bottomLeftChart" class="echarts_bottom"></Echart>
  </div>
</template>

<script>
import { currentGET } from "api";
import { graphic } from "echarts";
export default {
  data() {
    return {
      options: {},
    };
  },
  props: {},
  mounted() {
    this.getData();
  },
  methods: {
    getData() {
      this.pageflag = true;
      this.request.get("/hospital/region").then(res => {
        console.log("区域总览", res.data)
        const region = res.data[0]
        const region2023 = res.data[1]
        const total = region.东北区 + region.华北区 + region.华南区 + region.华中区 + region.西北区 + region.江苏区 + region.西南区 + region.华东区 + region.上海区
        const rate1 = (region.东北区 / total * 100).toFixed(2)
        const rate2 = (region.华北区 / total * 100).toFixed(2)
        const rate3 = (region.华南区 / total * 100).toFixed(2)
        const rate4 = (region.华中区 / total * 100).toFixed(2)
        const rate5 = (region.西北区 / total * 100).toFixed(2)
        const rate6 = (region.江苏区 / total * 100).toFixed(2)
        const rate7 = (region.西南区 / total * 100).toFixed(2)
        const rate8 = (region.华东区 / total * 100).toFixed(2)
        const rate9 = (region.上海区 / total * 100).toFixed(2)
        const data1 = {
          category: ['东北区', '华北区', '华南区', '华中区', '西北区', '江苏区', '西南区', '华东区', '上海区'],
          barData: [region2023.东北区, region2023.华北区, region2023.华南区, region2023.华中区, region2023.西北区, region2023.江苏区, region2023.西南区, region2023.华东区, region2023.上海区],  //今年安装
          lineData: [region.东北区, region.华北区, region.华南区, region.华中区, region.西北区, region.江苏区, region.西南区, region.华东区, region.上海区], //总数
          rateData: [rate1, rate2, rate3, rate4, rate5, rate6, rate7, rate8, rate9]   //占比

        }
        this.init(data1)
      })
      // const data1= {
      //   category: ['东北区','华北区','华南区','华中区','西北区','江苏区','西南区','华东区','上海区'],
      //   barData: ['10','20','30','40','50','60','70','50','20'],  //今年安装
      //   lineData: ['20','30','20','30','60','70','80','60','30'], //总数
      //   rateData:['5','20','10','30','20','20','10','5','20']   //占比

      // }
      // this.init(data1)
      // currentGET("big6", { companyName: this.companyName }).then((res) => {
      //   console.log("安装计划", res);
      //   if (res.success) {
      //     this.init(res.data);
      //   } else {
      //     this.pageflag = false;
      //     this.$Message({
      //       text: res.msg,
      //       type: "warning",
      //     });
      //   }
      // });
    },
    init(newData) {
      this.options = {
        tooltip: {
          trigger: "axis",
          backgroundColor: "rgba(0,0,0,.6)",
          borderColor: "rgba(147, 235, 248, .8)",
          textStyle: {
            color: "#FFF",
          },
          formatter: function (params) {
            // 添加单位
            var result = params[0].name + "<br>";
            params.forEach(function (item) {
              if (item.value) {
                if (item.seriesName == "安装率") {
                  result +=
                    item.marker +
                    " " +
                    item.seriesName +
                    " : " +
                    item.value +
                    "%</br>";
                } else {
                  result +=
                    item.marker +
                    " " +
                    item.seriesName +
                    " : " +
                    item.value +
                    "家</br>";
                }
              } else {
                result += item.marker + " " + item.seriesName + " :  - </br>";
              }
            });
            return result;
          },
        },
        legend: {
          // data: ["已安装", "计划安装", "安装率"],
          data: ["今年安装", "系统数量", "安装率"],
          textStyle: {
            color: "#B4B4B4",
          },
          top: "0",
        },
        grid: {
          left: "50px",
          right: "40px",
          bottom: "30px",
          top: "20px",
        },
        xAxis: {
          data: newData.category,
          axisLine: {
            lineStyle: {
              color: "#B4B4B4",
            },
          },
          axisTick: {
            show: false,
          },
        },
        yAxis: [
          {
            splitLine: { show: false },
            axisLine: {
              lineStyle: {
                color: "#B4B4B4",
              },
            },

            axisLabel: {
              formatter: "{value}",
            },
          },
          {
            splitLine: { show: false },
            axisLine: {
              lineStyle: {
                color: "#B4B4B4",
              },
            },
            axisLabel: {
              formatter: "{value}% ",
            },
          },
        ],
        series: [
          {
            name: "今年安装",
            type: "bar",
            barWidth: 10,
            itemStyle: {
              borderRadius: 5,
              color: new graphic.LinearGradient(0, 0, 0, 1, [
                { offset: 0, color: "#956FD4" },
                { offset: 1, color: "#3EACE5" },
              ]),
            },
            data: newData.barData,
          },
          {
            name: "系统数量",
            type: "bar",
            barGap: "-100%",
            barWidth: 10,
            itemStyle: {
              borderRadius: 5,
              color: new graphic.LinearGradient(0, 0, 0, 1, [
                { offset: 0, color: "rgba(156,107,211,0.8)" },
                { offset: 0.2, color: "rgba(156,107,211,0.5)" },
                { offset: 1, color: "rgba(156,107,211,0.2)" },
              ]),
            },
            z: -12,
            data: newData.lineData,
          },
          {
            name: "安装率",
            type: "line",
            smooth: true,
            showAllSymbol: true,
            symbol: "emptyCircle",
            symbolSize: 8,
            yAxisIndex: 1,
            itemStyle: {
              color: "#F02FC2",
            },
            data: newData.rateData,
          },
        ],
      };
    },
  },
};
</script>
<style lang="scss" scoped>
.center_bottom {
  width: 100%;
  height: 100%;

  .echarts_bottom {
    width: 100%;
    height: 100%;
  }
}
</style>
