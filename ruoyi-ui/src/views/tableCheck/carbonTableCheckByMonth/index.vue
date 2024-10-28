<template>
  <div class="carbon-monthly-report">
    <el-header>
      <h2>碳排放管理系统 - 碳排放月报</h2>
    </el-header>

    <el-row class="filter-row" type="flex" justify="start">
      <el-col :span="10">
        <el-date-picker
          v-model="selectedMonth"
          type="month"
          placeholder="选择月份"
          format="YYYY-MM"
          @change="fetchMonthlyReportData"
        ></el-date-picker>
      </el-col>
      <el-col :span="8">
        <el-button type="primary" @click="fetchMonthlyReportData">刷新报表</el-button>
        <el-button type="primary" @click="exportReport" style="margin-left: 10px;">导出PDF</el-button>
      </el-col>
    </el-row>

    <el-row class="summary" type="flex" justify="space-between">
      <el-col :span="6" class="summary-box">
        <el-card>
          <div>本月总排放量 (吨): {{ totalMonthlyEmission }}</div>
        </el-card>
      </el-col>
    </el-row>

    <el-row class="chart-row">
      <el-col :span="24">
        <div ref="monthlyChart" class="chart"></div>
      </el-col>
    </el-row>

    <div>
      <div class="chart-container">
        <div ref="fuelChart" class="chart_class"></div>
        <div ref="processChart" class="chart_class"></div>
      </div>
    </div>

    <div>
      <el-card>
        <h2>报告内容</h2>
        <el-table :data="tableData" border>
          <el-table-column prop="emission" label="排放量" sortable></el-table-column>
          <el-table-column prop="concentration" label="排放浓度" sortable></el-table-column>
          <el-table-column prop="quantityComparison" label="排放量对比" sortable></el-table-column>
          <el-table-column prop="intensityComparison" label="强度对比" sortable></el-table-column>
          <el-table-column prop="temperatureComparison" label="温度对比" sortable></el-table-column>
          <el-table-column prop="remainingQuantity" label="配额剩余量" sortable></el-table-column>
        </el-table>
      </el-card>
    </div>

  </div>
</template>

<script>
  import * as echarts from 'echarts';
  import jsPDF from 'jspdf';

  export default {
    data() {
      return {
        selectedMonth: '',
        monthlyReportData: [],
        totalMonthlyEmission: 0,
        compliantDays: 0,
        nonCompliantDays: 0,
        tableData: [
          {
            emission: '100',
            concentration: '50',
            quantityComparison: '10%',
            intensityComparison: '5%',
            temperatureComparison: '2%',
            remainingQuantity: '20',
          },
          {
            emission: '150',
            concentration: '75',
            quantityComparison: '15%',
            intensityComparison: '10%',
            temperatureComparison: '4%',
            remainingQuantity: '30',
          },
        ],
      };
    },
    methods: {
      fetchMonthlyReportData() {
        console.log('Fetching monthly report data for:', this.selectedMonth);

        // 示例数据，可替换为实际数据获取的API调用
        this.monthlyReportData = [
          {date: '2024-10-01', emission: 100.5, source: '生产', status: '合规', remarks: '运行正常'},
          {date: '2024-10-02', emission: 200.0, source: '运输', status: '合规', remarks: '运行正常'},
          {date: '2024-10-03', emission: 50.5, source: '办公', status: '未合规', remarks: '有设备掉线'},
          {date: '2024-10-04', emission: 0, source: '其他', status: '未合规', remarks: '无提交'},
          // 添加更多数据...
        ];

        this.calculateMonthlySummary();
        this.renderChart();
      },
      calculateMonthlySummary() {
        this.totalMonthlyEmission = this.monthlyReportData.reduce((sum, item) => sum + item.emission, 0).toFixed(2);
        this.compliantDays = this.monthlyReportData.filter(item => item.status === '合规').length;
        this.nonCompliantDays = this.monthlyReportData.filter(item => item.status === '未合规').length;
      },
      renderChart() {
        const chartDom = this.$refs.monthlyChart;
        const myChart = echarts.init(chartDom);
        const option = {
          title: {
            text: '碳排放量趋势',
          },
          tooltip: {
            trigger: 'axis',
          },
          xAxis: {
            type: 'category',
            data: this.monthlyReportData.map(item => item.date),
          },
          yAxis: {
            type: 'value',
            name: '碳排放量 (吨)',
          },
          series: [{
            name: '碳排放量 (吨)',
            type: 'line',
            data: this.monthlyReportData.map(item => item.emission),
            smooth: true,
            areaStyle: {}, // 填充效果
          }],
        };
        myChart.setOption(option);
      },
      exportReport() {
        const doc = new jsPDF();
        doc.text('碳排放月报', 20, 20); // 添加标题
        doc.text(`选择的月份: ${this.selectedMonth}`, 20, 30); // 选择的月份
        doc.save(`碳排放月报_${this.selectedMonth}.pdf`);
      },
      viewDetail(row) {
        alert(`查看详细信息: ${row.date}`);
      },
      editEntry(row) {
        alert(`编辑信息: ${row.date}`);
      },
      initTBData() {
        // 绘制第一个饼图: 排放来源占比
        const fuelChartInstance = echarts.init(this.$refs.fuelChart);
        const fuelOption = {
          title: {
            text: '排放来源',
            left: 'center',
          },
          tooltip: {
            trigger: 'item',
          },
          series: [{
            type: 'pie',
            radius: '50%',
            data: [
              {value: 45, name: '化石燃料燃烧'},
              {value: 30, name: '外购电力'},
              {value: 25, name: '含碳产品过程排放'},
            ],
            emphasis: {
              itemStyle: {
                shadowBlur: 10,
                shadowOffsetX: 0,
                shadowColor: 'rgba(0, 0, 0, 0.5)',
              },
            },
          }],
        };
        fuelChartInstance.setOption(fuelOption);

        // 绘制第二个饼图: 碳排放工序占比
        const processChartInstance = echarts.init(this.$refs.processChart);
        const processOption = {
          title: {
            text: '碳排放工序',
            left: 'center',
          },
          tooltip: {
            trigger: 'item',
          },
          series: [{
            type: 'pie',
            radius: '50%',
            data: [
              {value: 35, name: '焦化'},
              {value: 25, name: '烧结'},
              {value: 20, name: '球团'},
              {value: 15, name: '高炉炼铁'},
              {value: 25, name: '转炉炼钢'},
              {value: 55, name: '电炉炼钢'},
              {value: 45, name: '精炼'},
              {value: 35, name: '连铸'},
              {value: 25, name: '钢压延加工'},
              {value: 15, name: '石灰'},
            ],
            emphasis: {
              itemStyle: {
                shadowBlur: 10,
                shadowOffsetX: 0,
                shadowColor: 'rgba(0, 0, 0, 0.5)',
              },
            },
          }],
        };
        processChartInstance.setOption(processOption);
      }
    },
    mounted() {
      const currentMonth = new Date();
      const month = currentMonth.getMonth() + 1 < 10 ? `0${currentMonth.getMonth() + 1}` : currentMonth.getMonth() + 1;
      const year = currentMonth.getFullYear();
      this.selectedMonth = `${year}-${month}`;
      this.fetchMonthlyReportData(); // 初始加载报表数据
      this.initTBData()
    },
  };
</script>

<style scoped>
  .carbon-monthly-report {
    padding: 20px;
  }

  .filter-row {
    margin-bottom: 20px;
  }

  .summary {
    margin: 20px 0;
  }

  .summary-box {
    padding: 15px;
  }

  .chart-row {
    margin-top: 20px;
  }

  .chart {
    width: 100%;
    height: 400px;
    background-color: #f5f7fa;
  }

  .chart-container {
    width: 100%;
    margin: 20px auto;
    display: flex;
  }

  .chart_class {
    width: 50%;
    height: 400px;
  }
</style>
