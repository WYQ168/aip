<template>
  <div class="carbon-analysis-dashboard">
    <!-- 碳排放趋势折线图 -->
    <el-card class="trend-card" style="margin-top: 30px;">
      <div class="trend-header">
        <h3>碳排放趋势</h3>
        <div class="selectors">
          <el-select v-model="selectedYear" @change="updateChart" placeholder="选择年份">
            <el-option v-for="year in years" :key="year" :label="year" :value="year" />
          </el-select>
          <el-select v-model="selectedMarket" placeholder="选择市场">
            <el-option label="全国碳市场" value="national" />
            <el-option label="深圳市场" value="shenzhen" />
          </el-select>
        </div>
      </div>
      <div ref="trendChart" style="height: 400px;"></div>
    </el-card>
  </div>
</template>

<script>
import * as echarts from "echarts";

export default {
  data() {
    return {
      selectedYear: "2023年",
      selectedMarket: "national",
      years: ["2022年", "2023年", "2024年"],
      chartInstance: null,
      yearData: {
        "2022年": {
          排放量: [1000, 8000, 3000, 12000, 5000, 16000],
          配额量: [1500, 9000, 2000, 11000, 4000, 14000],
          深圳: [900, 10000, 2500, 13000, 4500, 15000],
        },
        "2023年": {
          排放量: [5000, 7000, 10000, 8000, 12000, 9000],
          配额量: [3000, 9000, 7000, 15000, 6000, 11000],
          深圳: [2000, 8000, 9000, 14000, 7000, 13000],
        },
        "2024年": {
          排放量: [8000, 2000, 12000, 3000, 15000, 5000],
          配额量: [7000, 3000, 14000, 4000, 13000, 6000],
          深圳: [6000, 4000, 10000, 2000, 11000, 3000],
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
      this.updateChart();
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
            smooth: true,
          },
          {
            name: "配额量",
            type: "line",
            data: data.配额量,
            itemStyle: { color: "#ff6347" },
            smooth: true,
          },
          {
            name: "Shenzhen",
            type: "line",
            data: data.深圳,
            itemStyle: { color: "#00ced1" },
            smooth: true,
          },
        ],
      };

      this.chartInstance.setOption(option);
    },
  },
};
</script>

<style scoped>
.trend-card {
  padding: 20px;
  border-radius: 8px;
}

.trend-header {
  display: flex;
  justify-content: space-between;
  align-items: center;
  margin-bottom: 10px;
}

.selectors {
  display: flex;
  gap: 10px;
}
</style>
