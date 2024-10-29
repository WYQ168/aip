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
      <el-table-column prop="activityData" label="活动数据名称" />
      <el-table-column prop="dataType" label="数据类型" />
      <el-table-column prop="relatedProcess" label="关联工序" />
      <el-table-column prop="source" label="数据来源" />
      <el-table-column prop="realData" label="数值" />
      <el-table-column prop="unit" label="单位" />
      <el-table-column prop="frequency" label="数据录入频率" />

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
        <el-form-item label="活动数据名称">
          <el-input v-model="editForm.activityData" placeholder="活动数据名称" />
        </el-form-item>
        <el-form-item label="数据类型">
          <el-input v-model="editForm.dataType" placeholder="数据类型" />
        </el-form-item>
        <el-form-item label="数据来源">
          <el-input v-model="editForm.dataSource" placeholder="数据来源" />
        </el-form-item>
        <el-form-item label="数量">
          <el-input v-model="editForm.amount" placeholder="数量" />
        </el-form-item>
        <el-form-item label="单位">
          <el-input v-model="editForm.unit" placeholder="单位" />
        </el-form-item>
        <el-form-item label="关联工序">
          <el-input v-model="editForm.relatedProcess" placeholder="关联工序" />
        </el-form-item>
        <el-form-item label="数据录入频率">
          <el-input v-model="editForm.dataEntryFrequency" placeholder="数据录入频率" />
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
        dataSource: '',
        realData: '',
        amount: '',
        unit: '',
        dataEntryFrequency: '',
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
      activityDataOptions: ['兰炭', '市政电','石灰石','天然气'],
      dataSourceOptions: ['手动输入','自动读取'],
      dataTypeOptions: ['化石燃料燃烧', '购入使用电力', '外供电力','购入使用电力','外供热力'],
      relatedProcessOptions: ['焦化', '烧结', '球团'],
      dataEntryFrequencyOptions: ['季', '月'],
      realDataOptions: ['296632.8', '1405576.663', '1083082.87', '80.12'],
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
        dataSource: this.getRandomValue(this.dataSourceOptions),
        realData: this.getRandomValue(this.realDataOptions),
        dataEntryFrequency: this.getRandomValue(this.dataEntryFrequencyOptions),
        source: '自动采集',
        unit: '吨',
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
