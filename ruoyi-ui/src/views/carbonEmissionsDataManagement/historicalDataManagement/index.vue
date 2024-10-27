<template>
  <div class="historical-data-manager">
    <h2>历史数据管理</h2>

    <!-- 日期选择和类型切换 -->
    <el-row :gutter="20" class="control-panel">
      <el-col :span="6">
        <el-date-picker
          v-model="selectedDate"
          type="daterange"
          range-separator="至"
          start-placeholder="开始日期"
          end-placeholder="结束日期"
          @change="fetchData"
        />
      </el-col>
      <el-col :span="18">
        <el-button-group>
          <el-button
            v-for="type in dataTypes"
            :key="type.value"
            :type="activeType === type.value ? 'primary' : 'default'"
            @click="changeType(type.value)"
            plain
          >
            {{ type.label }}
          </el-button>
        </el-button-group>
      </el-col>
    </el-row>

    <!-- 数据展示表格 -->
    <el-table :data="filteredData" border style="width: 100%; margin-top: 20px;">
      <el-table-column prop="name" label="数据项" />
      <el-table-column prop="value" label="数值" />
      <el-table-column prop="unit" label="单位" />
      <el-table-column prop="period" label="统计周期" />
      <el-table-column label="操作">
        <template #default="scope">
          <el-button type="text" @click="viewDetails(scope.row)">查看</el-button>
          <el-button type="text" @click="exportData(scope.row)">导出</el-button>
        </template>
      </el-table-column>
    </el-table>
  </div>
</template>

<script>
export default {
  data() {
    return {
      selectedDate: [],
      activeType: 'activityData',
      dataTypes: [
        { label: '活动数据', value: 'activityData' },
        { label: '排放因子', value: 'emissionFactor' },
        { label: '碳排放量', value: 'carbonEmission' },
        { label: '工序排放强度', value: 'processIntensity' },
        { label: '全厂排放强度', value: 'plantIntensity' },
        { label: '总碳足迹', value: 'totalCarbonFootprint' },
        { label: '碳资产平衡', value: 'carbonBalance' },
      ],
      tableData: {
        activityData: [
          { name: '煤炭消耗', value: 1000, unit: '吨', period: '月' },
          { name: '天然气消耗', value: 500, unit: '立方米', period: '日' },
        ],
        emissionFactor: [
          { name: '煤炭排放因子', value: 0.94, unit: 'tCO₂/t', period: '年' },
        ],
        carbonEmission: [
          { name: '全厂碳排放量', value: 3000, unit: 'tCO₂', period: '月' },
        ],
        processIntensity: [
          { name: '炼铁工序排放强度', value: 2.5, unit: 'tCO₂/吨', period: '年' },
        ],
        plantIntensity: [
          { name: '全厂排放强度', value: 1.8, unit: 'tCO₂/吨', period: '年' },
        ],
        totalCarbonFootprint: [
          { name: '产品碳足迹', value: 15000, unit: 'tCO₂', period: '年' },
        ],
        carbonBalance: [
          { name: '碳资产盈余', value: 1000, unit: '吨', period: '年' },
        ],
      },
    };
  },
  computed: {
    filteredData() {
      return this.tableData[this.activeType] || [];
    },
  },
  methods: {
    changeType(type) {
      this.activeType = type;
    },
    fetchData() {
      console.log('获取数据，日期范围：', this.selectedDate);
    },
    viewDetails(item) {
      console.log('查看详情:', item);
    },
    exportData(item) {
      console.log('导出数据:', item);
      // 可以集成下载功能，将数据导出为 XLS 等格式
    },
  },
};
</script>

<style scoped>
.historical-data-manager {
  padding: 20px;
}

.control-panel {
  margin-bottom: 10px;
}

.el-button {
  margin: 5px;
}

.el-table {
  margin-top: 20px;
}
</style>
