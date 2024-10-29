<template>
  <div class="carbon-analysis-dashboard">
    <!-- 碳排放强度诊断 -->
    <el-card class="diagnosis-card">
      <h2>碳排放强度诊断</h2>
      <div class="diagnosis-content">
        <div class="intensity-comparison">
          <div class="intensity-value">
            <p>当前强度</p>
            <hr />
            <p>强度配额</p>
          </div>
          <div class="intensity-value">
            <p><strong>120 tCO₂/t</strong></p>
            <hr />
            <p><strong>100 tCO₂/t</strong></p>
          </div>
        </div>
        <div class="alert-section">
          <el-icon><i class="el-icon-warning"></i></el-icon>
          <span class="alert-text">超标20%</span>
        </div>
      </div>
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

.diagnosis-card {
  margin-top: 20px;
  padding: 20px;
  background-color: #f7f7f7;
  border-radius: 8px;
  display: flex;
  align-items: center;
  justify-content: space-between;
}

.diagnosis-content {
  display: flex;
  align-items: center;
  justify-content: space-between;
  width: 100%;
}

.intensity-comparison {
  display: flex;
  align-items: center;
  justify-content: center;
  background-color: #e0e0e0;
  padding: 20px;
  border-radius: 8px;
  width:1000px;
  text-align: center;
}

.intensity-value p {
  margin: 0;
  font-size: 18px;
}

.alert-section {
  display: flex;
  align-items: center;
  justify-content: center;
  line-height: 112px;
  gap: 10px;
  color: #f5222d;
  font-size: 100px;
  letter-spacing: 40px; /* 增加字间距 */
}

.alert-text {

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
