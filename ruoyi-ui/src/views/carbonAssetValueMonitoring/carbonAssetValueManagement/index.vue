<template>
  <div class="carbon-dashboard">
    <!-- 碳资产价值展示 -->
    <div class="asset-value-container">
      <el-row :gutter="20">
        <el-col :span="12">
          <el-card class="value-card">
            <p>当前持有碳资产 (tCO2)</p>
            <b class="value-highlight">12,134,267</b>
          </el-card>
        </el-col>
        <el-col :span="12">
          <el-card class="value-card">
            <p>当前碳资产价值 (万元)</p>
            <b class="value-highlight">110.978</b>
          </el-card>
        </el-col>
      </el-row>
    </div>


  </div>
</template>

<script>
import * as echarts from "echarts";

export default {
  data() {
    return {
      selectedYear: "2024年",
      selectedMarket: "national",
      years: ["2022年", "2023年", "2024年"],
      chartInstance: null,
      yearData: {
        "2022年": {
          排放量: [3000, 2000, 2500, 2700, 3200, 3400],
          配额量: [2500, 1800, 2000, 2400, 3000, 3300],
          深圳: [2200, 2300, 2000, 2600, 2800, 3000],
        },
        "2023年": {
          排放量: [4000, 3000, 3500, 3700, 3900, 4100],
          配额量: [3000, 2500, 2700, 2900, 3300, 3700],
          深圳: [2500, 2700, 2900, 3000, 3200, 3500],
        },
        "2024年": {
          排放量: [5000, 4000, 4500, 4700, 4900, 5100],
          配额量: [4000, 3500, 3700, 3900, 4300, 4500],
          深圳: [3000, 3200, 3400, 3500, 3700, 4000],
        },
      },
    };
  },
  mounted() {
    this.initTrendChart();
  },
  methods: {
    initTrendChart() {
      const chartDom = this.$refs.trendChart;
      this.chartInstance = echarts.init(chartDom);

      this.updateChart(); // 初始化时加载默认年份数据
    },
    updateChart() {
      const year = this.selectedYear;
      const data = this.yearData[year];

      const option = {
        tooltip: { trigger: "axis" },
        legend: { data: ["排放量", "配额量", "Shenzhen"] },
        xAxis: {
          type: "category",
          data: ["1月", "3月", "5月", "7月", "9月", "11月"],
        },
        yAxis: { type: "value" },
        series: [
          {
            name: "排放量",
            type: "line",
            data: data.排放量,
            itemStyle: { color: "#8a2be2" },
          },
          {
            name: "配额量",
            type: "line",
            data: data.配额量,
            itemStyle: { color: "#ff6347" },
          },
          {
            name: "Shenzhen",
            type: "line",
            data: data.深圳,
            itemStyle: { color: "#00ced1" },
          },
        ],
      };

      this.chartInstance.setOption(option);
    },
  },
};
</script>

<style scoped>
.carbon-dashboard {
  padding: 20px;
}

.asset-value-container {
  margin-bottom: 20px;
}

.value-card {
  text-align: center;
  padding: 20px;
  background-color: #fff;
  border-radius: 8px;
  box-shadow: 0 2px 8px rgba(0, 0, 0, 0.1);
}

.value-highlight {
  font-size: 28px;
  color: #f5222d;
}


</style>
