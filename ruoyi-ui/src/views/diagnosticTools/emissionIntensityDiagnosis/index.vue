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
        <h3>工序碳排放强度诊断</h3>
      </div>
      <el-table :data="tableData" border style="width: 100%; margin-top: 10px;">
        <el-table-column prop="工序" label="工序" />
        <el-table-column prop="强度数据" label="强度数据" />
        <el-table-column prop="诊断" label="诊断" />
        <el-table-column prop="标杆水平" label="标杆水平" />
        <el-table-column prop="单位" label="单位" />
        <el-table-column prop="统计周期" label="统计周期" />
        <el-table-column prop="操作" label="操作" >
          <el-button type="text" icon="el-icon-view" @click="openDialog('edit', scope.$index)">查看</el-button>
          <el-button type="primary" @click="saveRecord">导出</el-button>
        </el-table-column>
      </el-table>
    </el-card>
  </div>
</template>

<script>
export default {
  data() {
    return {
      tableData: [
        { 工序: "烧结", 强度数据: "0.2130", 诊断: "↑", 标杆水平: "0.16", 单位:"t/t", 统计周期:"年", 操作: "查看"},
        { 工序: "球团", 强度数据: "0.1904", 诊断: "↑", 标杆水平: "0.21" , 单位:"t/t", 统计周期:"年", 操作:"查看"},
        { 工序: "高炉炼铁", 强度数据: "0.8625", 诊断: "" , 标杆水平: "1.56", 单位:"t/t", 统计周期:"年", 操作:"查看"},
        { 工序: "转炉炼钢", 强度数据: "-0.1780", 诊断: "↓" , 标杆水平: "-0.03", 单位:"t/t", 统计周期:"年", 操作:"查看"},
        { 工序: "精炼", 强度数据: "0.0220", 诊断: "" , 标杆水平: "0.04", 单位:"t/t", 统计周期:"年", 操作:"查看"},
        { 工序: "连铸", 强度数据: "0.0095", 诊断: "" , 标杆水平: "...", 单位:"t/t", 统计周期:"年", 操作:"查看"},
        { 工序: "钢压延加工", 强度数据: "0.3392", 诊断: "", 标杆水平: "...", 单位:"t/t", 统计周期:"年", 操作:"查看" },
        { 工序: "石灰", 强度数据: "0.5566", 诊断: "" , 标杆水平: "...", 单位:"t/t", 统计周期:"年", 操作:"查看"}
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
