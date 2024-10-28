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
      <el-table-column prop="activityData" label="活动数据" />
      <el-table-column prop="dataType" label="数据类型" />
      <el-table-column prop="relatedProcess" label="关联工序" />
      <el-table-column prop="source" label="数据来源" />
      <el-table-column prop="representation" label="数据代表性" />
      <el-table-column prop="format" label="数据格式" />
      <el-table-column prop="unit" label="数据单位" />
      <el-table-column prop="precision" label="有效数字" />
      <el-table-column prop="frequency" label="数据频率" />

      <el-table-column label="操作">
        <template #default="scope">
          <el-button type="text" @click="openEditDialog(scope.row)">编辑</el-button>
          <el-button type="text" style="color: red;" @click="deleteRow(scope.$index)">删除</el-button>
        </template>
      </el-table-column>
    </el-table>

    <!-- 编辑对话框 -->
    <el-dialog :visible.sync="dialogVisible" title="编辑行数据" width="30%">
      <el-form :model="editForm" label-width="120px">
        <el-form-item label="活动数据">
          <el-input v-model="editForm.activityData" placeholder="活动数据" />
        </el-form-item>
        <el-form-item label="数据类型">
          <el-input v-model="editForm.dataType" placeholder="数据类型" />
        </el-form-item>
        <el-form-item label="关联工序">
          <el-input v-model="editForm.relatedProcess" placeholder="关联工序" />
        </el-form-item>
      </el-form>

      <template #footer>
        <el-button @click="dialogVisible = false">取消</el-button>
        <el-button type="primary" @click="saveEdit">保存</el-button>
      </template>
    </el-dialog>
  </div>
</template>

<script>
export default {
  data() {
    return {
      dialogVisible: false,
      editForm: {
        activityData: '',
        dataType: '',
        relatedProcess: '',
      },
      currentRow: null,
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
        fossilFuel: [],
        productiveMaterial: [],
        externalElectricity: [],
        internalElectricity: [],
        externalHeat: [],
        internalHeat: [],
        carbonProduct: [],
      },
      activityDataOptions: ['煤炭', '矿石'],
      dataTypeOptions: ['化石燃料', '矿物质'],
      relatedProcessOptions: ['焦化', '烧结', '球团'],
    };
  },
  mounted() {
    this.initializeTableData();
  },
  computed: {
    filteredData() {
      return this.tableData[this.activeCategory] || [];
    },
  },
  methods: {
    initializeTableData() {
      for (let i = 0; i < 5; i++) {
        this.addRandomRow('fossilFuel');
        this.addRandomRow('productiveMaterial');
        this.addRandomRow('externalElectricity');
        this.addRandomRow('internalElectricity');
        this.addRandomRow('externalHeat');
        this.addRandomRow('internalHeat');
        this.addRandomRow('carbonProduct');
      }
    },
    addRandomRow(category) {
      const newRow = {
        activityData: this.getRandomValue(this.activityDataOptions),
        dataType: this.getRandomValue(this.dataTypeOptions),
        relatedProcess: this.getRandomValue(this.relatedProcessOptions),
        source: '自动采集',
        representation: '工序',
        format: 'CSV',
        unit: '吨',
        precision: Math.floor(Math.random() * 4) + 1,
        frequency: '月',
      };
      this.tableData[category].push(newRow);
    },
    getRandomValue(options) {
      const randomIndex = Math.floor(Math.random() * options.length);
      return options[randomIndex];
    },
    changeCategory(category) {
      this.activeCategory = category;
    },
    openEditDialog(row) {
      this.currentRow = row;
      this.editForm = { ...row };
      this.dialogVisible = true;
    },
    saveEdit() {
      Object.assign(this.currentRow, this.editForm);
      this.dialogVisible = false;
      console.log('保存后的数据:', this.currentRow);
    },
    deleteRow(index) {
      this.tableData[this.activeCategory].splice(index, 1);
      console.log(`删除了第 ${index + 1} 行`);
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
</style>
