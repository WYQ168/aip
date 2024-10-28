<template>
  <div class="carbon-analysis-dashboard">
    <!-- 标准强度和强度配额展示 -->
    <el-card class="dashboard-cards">
      <el-row :gutter="20">
        <el-col :span="12">
          <el-card class="metric-card">
            <h2 class="metric-title">当前的执行强度</h2>
            <p class="metric-value highlight-red">120%</p>
          </el-card>
        </el-col>
        <el-col :span="12">
          <el-card class="metric-card">
            <h2 class="metric-title">当前的强度配额</h2>
            <p class="metric-value highlight-red">800%</p>
          </el-card>
        </el-col>
      </el-row>
    </el-card>

    <!-- 碳排放表格 -->
    <el-card class="table-card" style="margin-top: 30px;">
      <div class="table-header">
        <h3>碳排放总量诊断</h3>
      </div>
      <el-table :data="tableData" border style="width: 100%; margin-top: 10px;">
        <el-table-column prop="项目" label="项目" />
        <el-table-column prop="数量" label="数量" />
        <el-table-column prop="单价" label="单价" />
      </el-table>
    </el-card>
  </div>
</template>

<script>
export default {
  data() {
    return {
      tableData: [
        { 项目: "当前碳排放总量", 数量: "10000", 单价: "" },
        { 项目: "碳排放配额", 数量: "8000", 单价: "" },
        { 项目: "CCER抵消量", 数量: "400", 单价: "" },
        { 项目: "绿电证抵消量", 数量: "600", 单价: "" },
        { 项目: "碳管惠", 数量: "?", 单价: "" },
        { 项目: "VCS", 数量: "?", 单价: "" },
        { 项目: "碳排放履约缺口", 数量: "1000", 单价: "" },
        { 项目: "总计", 数量: "xxx", 单价: "xxx" },
      ],
    };
  },
};
</script>

<style scoped>
.carbon-analysis-dashboard {
  padding: 20px;
}

.dashboard-cards {
  padding: 20px;
}

.metric-card {
  display: flex;
  flex-direction: column;
  align-items: center;
  justify-content: center;
  padding: 20px;
  background-color: #f7f7f7;
  border-radius: 8px;
  text-align: center;
}

.metric-title {
  font-weight: bold;
  margin-bottom: 10px;
}

.metric-value {
  font-size: 36px;
  margin: 0;
}

.highlight-red {
  color: #f5222d;
}

.table-card {
  margin-top: 20px;
  padding: 20px;
}

.table-header {
  display: flex;
  justify-content: space-between;
  align-items: center;
}
</style>
