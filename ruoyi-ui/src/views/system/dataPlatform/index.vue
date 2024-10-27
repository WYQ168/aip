<template>
  <div class="dashboard">
    <div class="row">
      <div class="chart-container">
        <echart-card title="User action by page" :option="userActionByPageOptions" />
      </div>
      <div class="chart-container">
        <echart-card title="Monthly Sales" :option="monthlySalesOptions" />
      </div>
    </div>

    <div class="row">
      <div class="chart-container">
        <echart-card title="User action by city" :option="userActionByCityOptions" />
      </div>
      <div class="chart-container">
        <echart-card title="Ticket Statistics" :option="ticketStatisticsOptions" />
      </div>
    </div>

    <div class="table-container">
      <customer-table :customers="customers" />
    </div>
  </div>
</template>

<script>
import * as echarts from 'echarts';
import EchartCard from './EchartCard.vue';
import CustomerTable from './CustomerTable.vue';

export default {
  components: {
    EchartCard,
    CustomerTable,
  },
  data() {
    return {
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
          data: ['Current Year', 'Past Year'],
          bottom: 0,
          icon: 'circle',
        },
        xAxis: {
          type: 'value',
          axisLine: { lineStyle: { color: '#ccc' } },
        },
        yAxis: {
          type: 'category',
          data: ['Mar', 'Apr', 'May', 'Jun', 'Jul', 'Aug', 'Sep', 'Oct', 'Nov', 'Dec'],
        },
        series: [
          {
            name: 'Current Year',
            type: 'bar',
            stack: 'Total',
            data: [1000, 2000, 3000, 4000, 5000, 6000, 7000, 8000, 9000, 10000],
            itemStyle: { color: '#7353BA' },
          },
          {
            name: 'Past Year',
            type: 'bar',
            stack: 'Total',
            data: [500, 1500, 2500, 3500, 4500, 5500, 6500, 7500, 8500, 9500],
            itemStyle: { color: '#E0E0E0' },
          },
        ],
      },

      userActionByCityOptions: {
        tooltip: { trigger: 'axis' },
        legend: {
          data: ['Beijing', 'Shanghai', 'Shenzhen'],
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
            name: 'Beijing',
            type: 'line',
            data: [4000, 5000, 6000, 5500, 4500, 4000, 5000],
            itemStyle: { color: '#7353BA' },
            smooth: true,
          },
          {
            name: 'Shanghai',
            type: 'line',
            data: [3000, 3500, 4000, 3000, 2500, 3000, 4000],
            itemStyle: { color: '#F47C7C' },
            smooth: true,
          },
          {
            name: 'Shenzhen',
            type: 'line',
            data: [2000, 2500, 3000, 2000, 1500, 2000, 3000],
            itemStyle: { color: '#5BC0BE' },
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
              { value: 546, name: 'Good Tickets', itemStyle: { color: '#7353BA' } },
              { value: 457, name: 'Bad Tickets', itemStyle: { color: '#F47C7C' } },
              { value: 386, name: 'Open Tickets', itemStyle: { color: '#5BC0BE' } },
              { value: 64, name: 'Solved Tickets', itemStyle: { color: '#E0E0E0' } },
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
</style>
