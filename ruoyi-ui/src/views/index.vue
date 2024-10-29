<template>
  <div class="dashboard">
    <div class="row">
      <div class="chart-container">
        <echart-card title="碳排放总量对比" :option="userActionByPageOptions" />
      </div>
      <div class="chart-container">
        <echart-card title="各工序碳排放量" :option="monthlySalesOptions" />
      </div>
    </div>

    <div class="row">
      <div class="chart-container">
        <echart-card title="碳排放趋势" :option="userActionByCityOptions" />
      </div>
      <div class="chart-container">
        <echart-card title="碳排放类型占比" :option="ticketStatisticsOptions" />
      </div>
    </div>

    <div class="table-container">
      <customer-table :customers="customers" />
    </div>
    <!-- 碳配额不足10%的弹窗 -->
    <el-dialog
      title="碳配额提醒"
      :visible.sync="quotaDialogVisible"
      width="30%"
      @close="handleDialogClose"
      class="centered-dialog"
    >
      <p>您的碳配额已不足 10%，请及时购买额外配额。</p>
      <template #footer>
        <el-button @click="quotaDialogVisible = false">关闭</el-button>
      </template>
    </el-dialog>
  </div>
</template>

<script>
import * as echarts from 'echarts';
import EchartCard from './dataPlatform/EchartCard.vue';
import CustomerTable from './dataPlatform/CustomerTable.vue';

export default {
  components: {
    EchartCard,
    CustomerTable,
  },
  data() {
    return {
      quotaDialogVisible: true, // 控制弹窗显示
      userActionByPageOptions: {
        tooltip: { trigger: 'axis' },
        legend: {
          data: ['Overall', 'Sidebar', 'Search'],
          bottom: 0,
          icon: 'circle',
        },
        xAxis: {
          type: 'category',
          data: ['Landing Page', 'Download Page', 'App Install', 'App Open'],
          axisLine: { lineStyle: { color: '#ccc' } },
        },
        yAxis: {
          type: 'value',
          splitLine: { lineStyle: { type: 'dashed' } },
        },
        series: [
          {
            name: 'Overall',
            type: 'bar',
            data: [5000, 3000, 2000, 4000],
            itemStyle: { color: '#7353BA' },
          },
          {
            name: 'Sidebar',
            type: 'bar',
            data: [4000, 2000, 1000, 3000],
            itemStyle: { color: '#F47C7C' },
          },
          {
            name: 'Search',
            type: 'bar',
            data: [3000, 1000, 500, 2000],
            itemStyle: { color: '#5BC0BE' },
          },
        ],
      },
      monthlySalesOptions: {
        tooltip: { trigger: 'axis' },
        legend: {
          data: ['今年', '去年'],
          bottom: 0,
          icon: 'circle',
        },
        xAxis: {
          type: 'value',
          axisLine: { lineStyle: { color: '#ccc' } },
        },
        yAxis: {
          type: 'category',
          data: ['烧结', '球团', '石灰', '高炉炼铁', '转炉炼钢', '精炼', '连铸'],
        },
        series: [
          {
            name: '今年',
            type: 'bar',
            stack: 'Total',
            data: [1000, 2000, 3000, 4000,300, 5000, 3590 ],
            itemStyle: { color: '#7353BA' },
          },
          {
            name: '去年',
            type: 'bar',
            stack: 'Total',
            data: [500, 1500, 2100, 3500, 400, 6480, 4012],
            itemStyle: { color: '#E0E0E0' },
          },
        ],
      },

      userActionByCityOptions: {
        tooltip: { trigger: 'axis' },
        legend: {
          data: [
            '化石燃料燃烧',
            '工业过程排放',
            '净购入电力排放',
            '固碳产品隐含排放',
            '净购热力排放',
            '二氧化碳排旋量'
          ],
          top: 0,
        },
        xAxis: {
          type: 'category',
          data: ['Sun', 'Mon', 'Tue', 'Wed', 'Thu', 'Fri', 'Sat'],
          axisLine: { lineStyle: { color: '#ccc' } },
        },
        yAxis: {
          type: 'value',
          splitLine: { lineStyle: { type: 'dashed' } },
        },
        series: [
          {
            name: '化石燃料燃烧',
            type: 'line',
            data: [5000, 6000, 7000, 6500, 6000, 5500, 6000],
            itemStyle: { color: '#7353BA' },
            smooth: true,
          },
          {
            name: '工业过程排放',
            type: 'line',
            data: [4000, 4500, 5000, 4000, 3500, 4000, 5000],
            itemStyle: { color: '#F47C7C' },
            smooth: true,
          },
          {
            name: '净购入电力排放',
            type: 'line',
            data: [-2000, -2500, -3000, -2000, -1500, -2000, -2500],
            itemStyle: { color: '#5BC0BE' },
            smooth: true,
          },
          {
            name: '净购热力排放',
            type: 'line',
            data: [1000, 1500, 2000, 1500, 1000, 1200, 1100],
            itemStyle: { color: '#FFA07A' },
            smooth: true,
          },
          {
            name: '固碳产品隐含排放',
            type: 'line',
            data: [3000, 3500, 4000, 3500, 3000, 2800, 2900],
            itemStyle: { color: '#20B2AA' },
            smooth: true,
          },
          {
            name: '二氧化碳排旋量',
            type: 'line',
            data: [6000, 6500, 7000, 6500, 6000, 5800, 5900],
            itemStyle: { color: '#FF4500' },
            smooth: true,
          },
        ],
      },

      ticketStatisticsOptions: {
        tooltip: { trigger: 'item' },
        legend: {
          orient: 'vertical',
          left: 'left',
        },
        series: [
          {
            name: 'Tickets',
            type: 'pie',
            radius: ['50%', '70%'],
            label: {
              show: true,
              position: 'center',
              formatter: '{b}\n{c}',
              fontSize: 24,
            },
            data: [
              { value: 926.48, name: '化石燃料燃烧排放', itemStyle: { color: '#5BC0BE' } },
              { value: 7.72, name: '净购热力排放', itemStyle: { color: '#7353BA' } },
              { value: 65.55, name: '净购入电力排放', itemStyle: { color: '#F47C7C' } },
              { value: 7.54, name: '固碳产品隐含排放', itemStyle: { color: '#E0E0E0' } },
              { value: 69.74, name: '工业过程排放', itemStyle: { color: '#a11c1c' } },
            ],
          },
        ],
      },
      customers: [
        {
          name: 'David Brown',
          id: '#239ILOU1103PP21',
          date: '12 Jan 2024',
          pack: 'Premium',
          amount: 568.0,
          status: 'Active'
        },
        {
          name: 'Eric Johnson',
          id: '#309PLOYK1212LM00',
          date: '19 Jan 2024',
          pack: 'Basic',
          amount: 0.0,
          status: 'Active'
        },
        {
          name: 'Lisa Miller',
          id: '#375TY1BM7834QA21',
          date: '21 Jan 2024',
          pack: 'Premium',
          amount: 908.0,
          status: 'Active'
        },
      ],
    };
  },
  methods: {
    handleDialogClose() {
      console.log('碳配额不足10%的弹窗已关闭');
    },
  }
};
</script>

<style>
.dashboard {
  display: flex;
  flex-direction: column;
  gap: 20px;
  padding: 20px;
  background-color: #fff;
}

.row {
  display: flex;
  gap: 20px;
}

.chart-container {
  flex: 1;
}

.table-container {
  margin-top: 20px;
}
.centered-dialog .el-dialog {
  margin: auto; /* 设置为自适应的 margin */
  transform: translateY(100%); /* 垂直方向上居中 */
}
</style>
