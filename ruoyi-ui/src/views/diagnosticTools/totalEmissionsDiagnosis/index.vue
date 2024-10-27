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
        <h3>碳排放总量记录</h3>
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
        <el-table-column prop="工艺表" label="碳排放工艺表" />
        <el-table-column prop="来源" label="来源" />
        <el-table-column prop="数据量" label="数据量" />
        <el-table-column prop="备注" label="备注" />
        <el-table-column label="操作" width="150">
          <template #default="scope">
            <el-button type="text" icon="el-icon-edit" @click="editRecord(scope.$index)"></el-button>
            <el-button type="text" icon="el-icon-delete" @click="deleteRecord(scope.$index)"></el-button>
          </template>
        </el-table-column>
      </el-table>
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

      const option = {
        tooltip: { trigger: "axis" },
        legend: { data: ["同比", "当前量", "环比"] },
        xAxis: {
          type: "category",
          data: this.months,
        },
        yAxis: { type: "value" },
        series: [
          { name: "同比", type: "bar", data: this.generateRandomData() },
          { name: "当前量", type: "bar", data: this.generateRandomData() },
          { name: "环比", type: "bar", data: this.generateRandomData() },
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
      const 工艺表 = ["焦化", "钢铁", "水泥", "电力", "化工", "冶金"];
      for (let i = 0; i < 10; i++) {
        records.push({
          工艺表: 工艺表[Math.floor(Math.random() * 工艺表.length)],
          来源: `数据来源 ${i + 1}`,
          数据量: Math.floor(Math.random() * 10000),
          备注: `备注信息 ${i + 1}`,
        });
      }
      return records;
    },
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
</style>
