<template>
  <div class="dynamic-table">
    <!-- 分类按钮组 -->
    <el-row :gutter="10" class="category-buttons">
      <el-col v-for="category in categories" :key="category.value" :span="3">
        <el-button
          :type="activeCategory === category.value ? 'primary' : 'default'"
          @click="changeCategory(category.value)"
          plain
        >
          {{ category.label }}
        </el-button>
      </el-col>
    </el-row>

    <!-- 表格 -->
    <el-table :data="filteredData" border style="width: 100%; margin-top: 20px;">
      <el-table-column prop="source" label="数据来源" />
      <el-table-column prop="representation" label="数据代表性" />
      <el-table-column prop="format" label="数据格式" />
      <el-table-column prop="unit" label="数据单位" />
      <el-table-column prop="precision" label="有效数字" />
      <el-table-column prop="frequency" label="数据频率" />
      <el-table-column label="操作">
        <template #default="scope">
          <el-button type="text" @click="viewItem(scope.row)">查看</el-button>
          <el-button type="text" @click="editItem(scope.row)">编辑</el-button>
        </template>
      </el-table-column>
    </el-table>
  </div>
</template>

<script>
export default {
  data() {
    return {
      activeCategory: 'fossilFuel',
      categories: [
        { label: '化石燃料', value: 'fossilFuel' },
        { label: '过程原料', value: 'productiveMaterial' },
        { label: '外购电力', value: 'externalElectricity' },
        { label: '自发电力', value: 'internalElectricity' },
        { label: '外购热力', value: 'externalHeat' },
        { label: '自产热力', value: 'internalHeat' },
        { label: '固碳产品', value: 'carbonProduct' },
      ],
      tableData: {
        fossilFuel: [
          { source: '自动采集', representation: '工序', format: 'CSV', unit: '吨', precision: 2, frequency: '日' },
          { source: '手动输入', representation: '全厂', format: 'XLS', unit: '立方米', precision: 3, frequency: '月' },
        ],
        productiveMaterial: [
          { source: '手动输入', representation: '车间', format: 'JSON', unit: '千克', precision: 2, frequency: '周' },
        ],
        externalElectricity: [
          { source: '自动采集', representation: '全厂', format: 'XLS', unit: 'MWh', precision: 1, frequency: '月' },
        ],
        internalElectricity: [
          { source: '自动采集', representation: '工序', format: 'CSV', unit: 'MWh', precision: 2, frequency: '日' },
        ],
        externalHeat: [
          { source: '手动输入', representation: '车间', format: 'JSON', unit: 'GJ', precision: 3, frequency: '月' },
        ],
        internalHeat: [
          { source: '自动采集', representation: '全厂', format: 'XLS', unit: 'GJ', precision: 1, frequency: '年' },
        ],
        carbonProduct: [
          { source: '手动输入', representation: '工序', format: 'CSV', unit: '吨', precision: 2, frequency: '周' },
        ],
      },
    };
  },
  computed: {
    filteredData() {
      return this.tableData[this.activeCategory] || [];
    },
  },
  methods: {
    changeCategory(category) {
      this.activeCategory = category;
    },
    viewItem(item) {
      console.log('查看:', item);
    },
    editItem(item) {
      console.log('编辑:', item);
    },
  },
};
</script>

<style scoped>
.dynamic-table {
  padding: 20px;
}

.category-buttons {
  margin-bottom: 10px;
}

.el-button {
  width: 100%;
}

.table-container {
  width: 100%;
  overflow-x: auto;
}
</style>
