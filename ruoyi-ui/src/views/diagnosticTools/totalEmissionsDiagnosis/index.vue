<template>
  <div class="carbon-record-dashboard">
    <!-- 碳排放总量对比图表 -->
    <el-card class="chart-card">
      <div class="chart-header">
        <h3><el-icon><i class="el-icon-data-analysis"></i></el-icon> 碳排放总量对比</h3>
        <p class="chart-subtitle">5 - step Funnel, Last 7 Days</p>
      </div>
      <div ref="barChart" style="height: 400px;"></div>
    </el-card>

    <!-- 碳排放总量记录表 -->
    <el-card class="table-card" style="margin-top: 30px;">
      <div class="table-header">
        <h3>碳排放活动类型</h3>
        <div class="table-selectors">
          <el-select v-model="selectedYear" @change="loadData" placeholder="选择年份">
            <el-option v-for="year in years" :key="year" :label="year" :value="year" />
          </el-select>
          <el-select v-model="selectedMonth" @change="loadData" placeholder="选择月份">
            <el-option v-for="(month, index) in months" :key="index" :label="month" :value="index + 1" />
          </el-select>
          <el-button icon="el-icon-search" plain @click="loadData">筛选</el-button>
        </div>
      </div>

      <el-table :data="tableData" border style="width: 100%; margin-top: 10px;">
        <el-table-column prop="工艺表" label=" " />
        <el-table-column prop="一月" label="1月" />
        <el-table-column prop="二月" label="2月" />
        <el-table-column prop="三月" label="3月" />
        <el-table-column prop="四月" label="4月" />
        <el-table-column prop="五月" label="5月" />
        <el-table-column prop="六月" label="6月" />
        <el-table-column prop="七月" label="7月" />
        <el-table-column prop="八月" label="8月" />
        <el-table-column prop="九月" label="9月" />
        <el-table-column prop="十月" label="10月" />
        <el-table-column prop="十一月" label="11月" />
        <el-table-column prop="十二月" label="12月" />
      </el-table>
    </el-card>
<br/>

    <el-card class="table-card" style="margin-top: 30px;">
      <h3>工序碳排放量</h3>
    <div>
      <el-table :data="tableData2" style="width: 100%;">
        <el-table-column prop="process" label=" "></el-table-column>
        <el-table-column prop="jan" label="1月"></el-table-column>
        <el-table-column prop="feb" label="2月"></el-table-column>
        <el-table-column prop="mar" label="3月"></el-table-column>
        <el-table-column prop="apr" label="4月"></el-table-column>
        <el-table-column prop="may" label="5月"></el-table-column>
        <el-table-column prop="more" label="......"></el-table-column>
      </el-table>
    </div>
    </el-card>
  </div>


</template>

<script>
import * as echarts from "echarts";

export default {
  data() {
    return {
      selectedYear: new Date().getFullYear(),
      selectedMonth: new Date().getMonth() + 1,
      years: ["2022年", "2023年", "2024年"],
      months: ["1月", "2月", "3月", "4月", "5月", "6月", "7月", "8月", "9月", "10月", "11月", "12月"],
      tableData: [],
      tableData2: [
        { process: '烧结', jan: 528487, feb: 62609, mar: 113736, apr: 131658, may: 128173 ,more: '......'},
        { process: '球团', jan: 17429, feb: 17121, mar: 20644, apr: 27090, may: 29077 ,more: '......'},
        { process: '高炉炼铁', jan: 190948, feb: 141689, mar: 337233, apr: 399810, may: 454660 ,more: '......'},
        { process: '转炉炼钢', jan: -48442, feb: -30128, mar: -67550, apr: -75349, may: -84706 ,more: '......'},
        { process: '精炼', jan: 7857, feb: 5989, mar: 8237, apr: 10285, may: 11682 ,more: '......'},
        { process: '连铸', jan: 3461, feb: 2624, mar: 3553, apr: 4447, may: 5025 ,more: '......'},
        { process: '钢坯延加工', jan: 124056, feb: 100216, mar: 22437, apr: 155762, may: 177975 ,more: '......'},
        { process: '石灰', jan: 29049, feb: 20983, mar: 31656, apr: 39308, may: 39637 ,more: '......'},
        { process: '全部工序CO2排放量', jan: 852845, feb: 321103, mar: 469945, apr: 693011, may: 761523 ,more: '......'}
      ],
      allData: this.generateAllData(), // 生成所有年份和月份的数据
    };
  },
  mounted() {
    this.initBarChart();
    this.loadData(); // 初始加载数据
  },
  methods: {
    initBarChart() {
      const chartDom = this.$refs.barChart;
      const myChart = echarts.init(chartDom);

      // 生成随机数据的函数
      const generateRandomData = (min, max) => {
        return Math.floor(Math.random() * (max - min + 1)) + min;
      };

      const months = ['1月', '2月', '3月', '4月', '5月', '6月', '7月', '8月', '9月', '10月'];
      const yearOnYearData = Array.from({ length: 10 }, () => generateRandomData(50000, 200000));
      const monthOnMonthData = Array.from({ length: 10 }, () => generateRandomData(50000, 200000));

      // 生成更平滑的环比增长率数据
      const growthRateData = [];
      let lastValue = 0; // 上一个值
      for (let i = 0; i < 10; i++) {
        const change = generateRandomData(-5, 5); // 控制变化幅度
        lastValue += change; // 累加得出新的值
        growthRateData.push(lastValue);
      }

      const option = {
        tooltip: {
          trigger: 'axis',
          axisPointer: { type: 'cross' },
        },
        legend: {
          data: ['同比', '环比', '环比增长率'],
          top: '5%',
          left: 'center',
        },
        xAxis: {
          type: 'category',
          data: months,
          axisLine: { lineStyle: { color: '#ccc' } },
        },
        yAxis: [
          {
            type: 'value',
            name: '碳排放量(tCO2)',
            position: 'left',
            axisLine: { lineStyle: { color: '#5470C6' } },
            splitLine: { lineStyle: { type: 'dashed' } },
          },
          {
            type: 'value',
            name: '环比增长率 (%)',
            position: 'right',
            axisLine: { lineStyle: { color: '#91CC75' } },
            splitLine: { show: false },
            axisLabel: {
              formatter: '{value}%',
            },
          },
        ],
        series: [
          {
            name: '同比',
            type: 'bar',
            data: yearOnYearData,
            itemStyle: { color: '#5470C6' },
            barWidth: '20%', // 调整柱状图宽度，使其更细
            barGap: '30%', // 增加柱状图之间的间隔
          },
          {
            name: '环比',
            type: 'bar',
            data: monthOnMonthData,
            itemStyle: { color: '#FFA500' },
            barWidth: '20%',
            barGap: '30%',
          },
          {
            name: '环比增长率',
            type: 'line',
            yAxisIndex: 1,
            data: growthRateData,
            itemStyle: { color: '#91CC75' },
            label: {
              show: true,
              position: 'top',
              formatter: '{c}%',
            },
            smooth: true,
          },
        ],
      };

      myChart.setOption(option);
    },

    loadData() {
      const year = this.selectedYear;
      const month = this.selectedMonth;
      this.tableData = this.allData[`${year}-${month}`] || [];
    },
    generateAllData() {
      const data = {};
      for (let year = 2022; year <= 2024; year++) {
        for (let month = 1; month <= 12; month++) {
          const key = `${year}-${month}`;
          data[key] = this.generateRandomTableData();
        }
      }
      return data;
    },
    generateRandomTableData() {
  const records = [];
  const 工艺表 = [
    "化石燃料燃烧排放",
    "工业过程排放",
    "净购入电力排放",
    "净购热力排放",
    "固碳产品隐含排放",
    "碳排放总量"
  ];
  const data = [
    [641128.7, 551701.8, 725216.4, 877698.9, 972821.7, 893147.4, 873880.1, 951481.7, 952006.1, 920070.1,767932.7,499076.19],
    [40053.67, 27802.79, 45775.44, 66979.3, 68060.87, 62496.83, 64599.76, 59817.75, 53821.17, 56307.11,51144.99,40574.38],
    [35893.68, 36951.99, 56340.69, 62160.65, 66955.66, 57967.74, 54665.1, 50284, 47230.3, 55103.3, 64128.29, 67845.31],
    [-17417.6, -14128.08, -8511.56, -2505.08, -2411.82, -2101.9, -2172.67, -2516.79, -2517.77, -2586.88, -5657.32,-14686.65],
    [5564.81, 4733.38, 5725, 6783.02, 7828.49, 6743.89, 6607.86, 7398.01, 7160.85, 7185.02, 6042.16 ,3702.22],
    [694094, 597595, 813096, 997551, 1097598, 1004766, 984364, 1051669, 1043379, 1021709,871507,589107]
  ];

  for (let i = 0; i < 工艺表.length; i++) {
    records.push({
      工艺表: 工艺表[i],
      一月: data[i][0],
      二月: data[i][1],
      三月: data[i][2],
      四月: data[i][3],
      五月: data[i][4],
      六月: data[i][5],
      七月: data[i][6],
      八月: data[i][7],
      九月: data[i][8],
      十月: data[i][9],
      十一月: data[i][10],
      十二月: data[i][11],
    });
  }

  return records;
}
,
    generateRandomData() {
      return Array.from({ length: 12 }, () => Math.floor(Math.random() * 6000));
    },
    editRecord(index) {
      console.log("编辑记录：", index);
      // 添加编辑逻辑
    },
    deleteRecord(index) {
      this.tableData.splice(index, 1);
      console.log("删除记录：", index);
    },
  },
};
</script>

<style scoped>
.carbon-record-dashboard {
  padding: 20px;
}

.chart-card {
  padding: 20px;
  border-radius: 8px;
}

.chart-header {
  display: flex;
  justify-content: space-between;
  align-items: center;
  margin-bottom: 10px;
}

.table-card {
  margin-top: 20px;
  padding: 20px;
  border-radius: 8px;
}

.table-header {
  display: flex;
  justify-content: space-between;
  align-items: center;
}

.table-selectors {
  display: flex;
  gap: 10px;
}

.el-table {
  margin-top: 10px;
}
.el-table th, .el-table td {
  text-align: center;
}
</style>
