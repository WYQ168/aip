<template>
  <div class="carbon-annual-report">
    <el-header>
      <h2>碳排放管理系统 - 碳排放年报</h2>
    </el-header>

    <el-row class="filter-row" type="flex" justify="start">
      <el-col :span="10">
        <el-select v-model="selectedYear" placeholder="选择年份" @change="fetchAnnualReportData">
          <el-option
            v-for="year in yearOptions"
            :key="year"
            :label="year"
            :value="year">
          </el-option>
        </el-select>
      </el-col>
      <el-col :span="8">
        <el-button type="primary" @click="fetchAnnualReportData">刷新报表</el-button>
        <el-button type="primary" @click="exportReport" style="margin-left: 10px;">导出PDF</el-button>
      </el-col>
    </el-row>

    <el-row class="summary" type="flex" justify="space-between">
      <el-col :span="6" class="summary-box">
        <el-card>
          <div>年度总排放量 (吨): {{ totalAnnualEmission }}</div>
        </el-card>
      </el-col>
      <el-col :span="6" class="summary-box">
        <el-card>
          <div>合规天数: {{ compliantDays }}</div>
        </el-card>
      </el-col>
      <el-col :span="6" class="summary-box">
        <el-card>
          <div>未合规天数: {{ nonCompliantDays }}</div>
        </el-card>
      </el-col>
    </el-row>

    <el-row class="chart-row">
      <el-col :span="24">
        <div ref="annualChart" class="chart"></div>
      </el-col>
    </el-row>

    <el-table :data="annualReportData" stripe style="width: 100%">
      <el-table-column prop="month" label="月份" width="150" />
      <el-table-column prop="monthlyEmission" label="月排放量 (吨)" width="180" />
      <el-table-column prop="status" label="状态" width="100" />
      <el-table-column prop="remarks" label="备注" width="200" />
      <el-table-column label="操作" width="150">
        <template v-slot="scope">
          <el-button type="success" @click="viewDetail(scope.row)">查看</el-button>
          <el-button type="warning" @click="editEntry(scope.row)">编辑</el-button>
        </template>
      </el-table-column>
    </el-table>
  </div>
</template>

<script>
  import * as echarts from 'echarts';
  import jsPDF from 'jspdf';

  export default {
    data() {
      return {
        selectedYear: '',
        yearOptions: Array.from({ length: 11 }, (_, i) => new Date().getFullYear() - i),
        annualReportData: [],
        totalAnnualEmission: 0,
        compliantDays: 0,
        nonCompliantDays: 0,
      };
    },
    methods: {
      fetchAnnualReportData() {
        console.log('Fetching annual report data for:', this.selectedYear);

        // 示例数据，可替换为实际数据获取的API调用
        this.annualReportData = [
          { month: '2024-01', monthlyEmission: 100.5, status: '合规', remarks: '运行正常' },
          { month: '2024-02', monthlyEmission: 200.0, status: '合规', remarks: '运行正常' },
          { month: '2024-03', monthlyEmission: 150.0, status: '合规', remarks: '处理问题' },
          { month: '2024-04', monthlyEmission: 90.0, status: '未合规', remarks: '能源浪费' },
          { month: '2024-05', monthlyEmission: 70.3, status: '合规', remarks: '监控显示正常' },
          { month: '2024-06', monthlyEmission: 110.6, status: '合规', remarks: '设备运行良好' },
          { month: '2024-07', monthlyEmission: 130.1, status: '合规', remarks: '例行检查完成' },
          { month: '2024-08', monthlyEmission: 80.7, status: '合规', remarks: '监测正常' },
          { month: '2024-09', monthlyEmission: 90.5, status: '未合规', remarks: '报告延迟' },
          { month: '2024-10', monthlyEmission: 140.0, status: '合规', remarks: '正常' },
          { month: '2024-11', monthlyEmission: 125.3, status: '合规', remarks: '运行正常' },
          { month: '2024-12', monthlyEmission: 95.8, status: '未合规', remarks: '减排措施不足' },
        ];

        this.calculateAnnualSummary();
        this.renderChart();
      },
      calculateAnnualSummary() {
        this.totalAnnualEmission = this.annualReportData.reduce((sum, item) => sum + item.monthlyEmission, 0).toFixed(2);
        this.compliantDays = this.annualReportData.filter(item => item.status === '合规').length;
        this.nonCompliantDays = this.annualReportData.filter(item => item.status === '未合规').length;
      },
      renderChart() {
        const chartDom = this.$refs.annualChart;
        const myChart = echarts.init(chartDom);
        const option = {
          title: {
            text: `${this.selectedYear} 年碳排放量趋势`,
          },
          tooltip: {
            trigger: 'axis',
          },
          legend: {
            data: ['月排放量 (吨)'],
          },
          xAxis: {
            type: 'category',
            data: this.annualReportData.map(item => item.month),
          },
          yAxis: {
            type: 'value',
            name: '月排放量 (吨)',
          },
          series: [{
            name: '月排放量 (吨)',
            type: 'line',
            data: this.annualReportData.map(item => item.monthlyEmission),
            smooth: true,
          }],
        };
        myChart.setOption(option);
      },
      exportReport() {
        const doc = new jsPDF();
        doc.text('碳排放年报', 20, 20); // 添加标题
        doc.text(`选择的年份: ${this.selectedYear}`, 20, 30); // 选择的年份
        doc.save(`碳排放年报_${this.selectedYear}.pdf`);
      },
      viewDetail(row) {
        alert(`查看详细信息: ${row.month}`);
      },
      editEntry(row) {
        alert(`编辑信息: ${row.month}`);
      },
    },
    mounted() {
      this.selectedYear = new Date().getFullYear();
      this.fetchAnnualReportData(); // 初始加载报表数据
    },
  };
</script>

<style scoped>
  .carbon-annual-report {
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
</style>
