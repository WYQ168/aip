<template>
  <div class="activity-emission-table">
    <h2>活动数据与排放因子计算</h2>

    <!-- 添加新记录按钮 -->
    <el-button type="primary" icon="el-icon-plus" @click="addRow" style="margin-bottom: 10px;">
      添加新记录
    </el-button>

    <!-- 数据表格 -->
    <el-table :data="tableData" border style="width: 100%;">
      <el-table-column prop="index" label="序号"/>

      <!-- 固定活动数据列 -->
      <el-table-column prop="activity" label="选择活动数据" />

      <el-table-column label="选择排放因子">
        <template #default="scope">
          <el-select
            v-model="scope.row.factor"
            placeholder="选择排放因子"
            style="width: 100%;"
            @change="generateResult(scope.row)"
          >
            <el-option
              v-for="factor in emissionFactors"
              :key="factor.value"
              :label="factor.label"
              :value="factor.value"
            />
          </el-select>
        </template>
      </el-table-column>

      <el-table-column prop="result" label="计算结果" width="180" />

      <el-table-column label="操作" width="180">
        <template #default="scope">
          <el-button type="text" @click="editRow(scope.$index)">编辑</el-button>
          <el-button type="text" style="color: red;" @click="deleteRow(scope.$index)">删除</el-button>
        </template>
      </el-table-column>
    </el-table>
  </div>
</template>

<script>
export default {
  data() {
    return {
      tableData: [],
      emissionFactors: [
        { label: '煤炭排放因子', value: 'coal' },
        { label: '天然气排放因子', value: 'naturalGas' },
        { label: '电力排放因子', value: 'electricity' },
        { label: '热力排放因子', value: 'heat' },
        { label: '碳捕获排放因子', value: 'carbonCapture' },
      ],
    };
  },
  mounted() {
    this.initializeTableData();
  },
  methods: {
    initializeTableData() {
      // 初始化 20 条示例数据，随机分配 "外购电力1" 或 "外购电力2"
      for (let i = 1; i <= 15; i++) {
        this.tableData.push({
          index: i,
          activity: Math.random() > 0.5 ? '外购电力1' : '外购电力2',
          factor: this.emissionFactors[Math.floor(Math.random() * this.emissionFactors.length)].value,
          result: `${(Math.random() * 100).toFixed(2)} tCO₂`,
        });
      }
    },
    addRow() {
      const newIndex = this.tableData.length + 1;
      this.tableData.push({
        index: newIndex,
        activity: Math.random() > 0.5 ? '外购电力1' : '外购电力2',
        factor: '',
        result: '',
      });
    },
    editRow(index) {
      console.log('编辑第', index + 1, '行');
      // 可在此实现具体的编辑逻辑
    },
    deleteRow(index) {
      this.tableData.splice(index, 1);
      console.log('删除第', index + 1, '行');
    },
    generateResult(row) {
      // 如果排放因子已选择，则生成随机结果
      if (row.factor) {
        row.result = `${(Math.random() * 100).toFixed(2)} tCO₂`;
      }
    },
  },
};
</script>

<style scoped>
.activity-emission-table {
  padding: 20px;
}

.el-button {
  margin: 5px 0;
}

.el-table {
  margin-top: 10px;
}
</style>
