<template>
  <div class="echart-card">
    <div class="header">
      <h3>{{ title }}</h3>
      <div class="dropdowns">
        <select>
          <option>Monthly</option>
          <option>Weekly</option>
        </select>
      </div>
    </div>
    <div ref="chart" class="chart"></div>
  </div>
</template>

<script>
import * as echarts from 'echarts';

export default {
  props: ['title', 'option'],
  mounted() {
    this.chart = echarts.init(this.$refs.chart);
    this.chart.setOption(this.option);
    window.addEventListener('resize', () => this.chart.resize());
  },
  watch: {
    option(newOption) {
      this.chart.setOption(newOption);
    },
  },
  beforeDestroy() {
    window.removeEventListener('resize', this.chart.resize);
  },
};
</script>

<style scoped>
.echart-card {
  border: 1px solid #e5e5e5;
  border-radius: 8px;
  background-color: white;
  padding: 20px;
  box-shadow: 0 2px 4px rgba(0, 0, 0, 0.1);
  margin-bottom: 20px;
}

.header {
  display: flex;
  justify-content: space-between;
  align-items: center;
  margin-bottom: 10px;
}

.chart {
  width: 100%;
  height: 300px;
}

select {
  padding: 5px;
  border: 1px solid #ccc;
  border-radius: 5px;
}
</style>
