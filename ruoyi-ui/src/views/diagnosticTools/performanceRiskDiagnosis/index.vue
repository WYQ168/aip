<template>
  <div class="compliance-dashboard">
    <!-- 履约成本分析 -->
    <el-card class="analysis-section">
      <h2><el-icon><i class="el-icon-pie-chart"></i></el-icon> 履约成本分析</h2>
      <el-row :gutter="20" class="metrics-row">
        <el-col :span="12">
          <el-card class="metric-card">
            <h3>当前履约缺口（tCO2）</h3>
            <p class="metric-value highlight-red">12,134,267</p>
          </el-card>
        </el-col>
        <el-col :span="12">
          <el-card class="metric-card">
            <h3>当前履约成本（万元）</h3>
            <p class="metric-value highlight-red">110.978</p>
          </el-card>
        </el-col>
      </el-row>
    </el-card>

    <!-- 智能履约建议 -->
    <el-card class="suggestion-section" style="margin-top: 30px;">
      <h2><el-icon><i class="el-icon-guide"></i></el-icon> 智能履约建议</h2>
      <el-row :gutter="20" class="suggestion-group">
        <el-col :span="24">
          <el-card class="suggestion-card">
            <p class="suggestion">建议组合一：() CEA + () CCER + () 绿电</p>
          </el-card>
        </el-col>
        <el-col :span="24">
          <el-card class="suggestion-card">
            <p class="suggestion">建议组合二：() CEA + () CCER + () 绿电 + () VCS</p>
          </el-card>
        </el-col>
        <el-col :span="24">
          <el-card class="suggestion-card">
            <p class="suggestion">建议组合三：() CEA + () CCER + () 绿电 + () 碳普惠</p>
          </el-card>
        </el-col>
      </el-row>
    </el-card>
  </div>
</template>

<script>
export default {
  data() {
    return {
      // 数据可以根据需要进行动态加载或编辑
    };
  },
};
</script>

<style scoped>
.compliance-dashboard {
  padding: 20px;
}

.analysis-section {
  padding: 20px;
  border-radius: 8px;
}

.metrics-row {
  margin-top: 10px;
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

.metric-value {
  font-size: 36px;
  margin: 0;
}

.highlight-red {
  color: #f5222d;
}

.suggestion-section {
  padding: 20px;
  border-radius: 8px;
}

.suggestion-group {
  margin-top: 10px;
}

.suggestion-card {
  margin: 10px 0;
  padding: 15px;
  text-align: center;
  background-color: #f0f0f0;
  border-radius: 8px;
}

.suggestion {
  font-size: 24px;
  font-weight: bold;
  color: #f5222d;
}
</style>
