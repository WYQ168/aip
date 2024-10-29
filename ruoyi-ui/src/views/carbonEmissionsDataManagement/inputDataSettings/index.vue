<template>
  <div class="emission-factor-manager">
    <!-- 搜索栏 -->
    <el-row :gutter="20" class="search-bar">
      <el-col :span="8">
        <el-input v-model="searchName" placeholder="按设置项搜索" clearable />
      </el-col>
      <el-col :span="8">
        <el-input v-model="searchCode" placeholder="按设置内容搜索" clearable />
      </el-col>
      <el-col :span="8" class="search-buttons">
        <el-button type="primary" @click="searchFactors">搜索</el-button>
        <el-button @click="resetSearch">重置</el-button>
      </el-col>
    </el-row>

    <!-- 新增按钮 -->
    <el-button type="success" icon="el-icon-plus" @click="openAddDialog" style="margin: 10px 0;">
      新增数据项
    </el-button>

    <!-- 表格容器 -->
    <div class="table-container">
      <el-table :data="filteredFactors" border style="width: 100%;">
        <el-table-column type="selection" width="55"></el-table-column>
        <el-table-column prop="settingItem" label="项目(数据类型)" />
        <el-table-column prop="settingContent" label="设置内容" />
        <el-table-column label="操作">
          <template #default="scope">
            <el-button type="text" @click="editFactor(scope.row)">编辑</el-button>
            <el-button type="text" @click="deleteFactor(scope.row)" style="color: red;">删除</el-button>
          </template>
        </el-table-column>
      </el-table>
    </div>

    <!-- 新增/编辑对话框 -->
    <el-dialog :title="isEdit ? '编辑设置项' : '新增设置项'" :visible.sync="dialogVisible">
      <el-form :model="currentFactor" label-width="120px">
        <el-form-item label="有效数字" prop="selectField">
          <el-select v-model="currentFactor.selectField" placeholder="请选择" >
            <el-option
              v-for="item in options1"
              :key="item.value"
              :label="item.label"
              :value="item.value">
            </el-option>
          </el-select>
        </el-form-item>
        <el-form-item label="数据来源" prop="selectField">
          <el-select v-model="currentFactor.selectField" placeholder="请选择" >
            <el-option
              v-for="item in options2"
              :key="item.value"
              :label="item.label"
              :value="item.value">
            </el-option>
          </el-select>
        </el-form-item>
        <el-form-item label="数据阈值" prop="selectField">
          <el-select v-model="currentFactor.selectField" placeholder="请选择" >
            <el-option
              v-for="item in options"
              :key="item.value"
              :label="item.label"
              :value="item.value">
            </el-option>
          </el-select>
        </el-form-item>
        <el-form-item label="数据单位" prop="selectField">
          <el-select v-model="currentFactor.selectField" placeholder="请选择" >
            <el-option
              v-for="item in options4"
              :key="item.value"
              :label="item.label"
              :value="item.value">
            </el-option>
          </el-select>
        </el-form-item>
        <el-form-item label="录入频率" prop="selectField">
          <el-select v-model="currentFactor.selectField" placeholder="请选择" >
            <el-option
              v-for="item in options5"
              :key="item.value"
              :label="item.label"
              :value="item.value">
            </el-option>
          </el-select>
        </el-form-item>
      </el-form>
      <template #footer>
        <el-button @click="cancelDialog">取消</el-button>
        <el-button type="primary" @click="saveFactor" :disabled="!isFormValid">保存</el-button>
      </template>
    </el-dialog>
  </div>
</template>

<script>
export default {
  data() {
    return {
      searchName: '',
      searchCode: '',
      factors: this.generateRandomData(),
      dialogVisible: false,
      isEdit: false,
      editIndex: null,
      currentFactor: {
        settingItem: '',
        settingContent: '',
        selectField: '',
      },

      options: [ // 下拉框的选项列表
        { value: 'option1', label: '50' },
        { value: 'option2', label: '100' },
        { value: 'option3', label: '200' }
      ],

      options1: [ // 下拉框的选项列表
        { value: 'option1', label: '1' },
        { value: 'option2', label: '2' },
        { value: 'option3', label: '3' }
      ],
      options2: [ // 下拉框的选项列表
        { value: 'option1', label: '手动输入' },
        { value: 'option2', label: '自动采集' },
      ],
      options4: [ // 下拉框的选项列表
        { value: 'option1', label: '吨(t)' },
        { value: 'option2', label: '万立方米(10^4 Nm3)' },
        { value: 'option2', label: '千克(kg)' },
        { value: 'option2', label: '兆瓦时(MWh)' },
        { value: 'option2', label: '吉焦(GJ)' },
      ],
      options5: [ // 下拉框的选项列表
        { value: 'option1', label: '日' },
        { value: 'option2', label: '月' },
        { value: 'option2', label: '季' },
        { value: 'option2', label: '年' },
      ],
    };
  },
  computed: {
    filteredFactors() {
      return this.factors.filter(
        (factor) =>
          (!this.searchName || factor.settingItem.includes(this.searchName)) &&
          (!this.searchCode || factor.settingContent.includes(this.searchCode))
      );
    },
    isFormValid() {
      return this.currentFactor.settingItem && this.currentFactor.settingContent;
    },
  },
  methods: {
    generateRandomData() {
      const settingItems = [
        '兰炭', '市政电'
      ];
      const settingContents = [
        '已编辑', '缺省'
      ];
      const randomFactors = Array.from({ length: 5 }, () => ({
        settingItem: settingItems[Math.floor(Math.random() * settingItems.length)],
        settingContent: settingContents[Math.floor(Math.random() * settingContents.length)],
      }));
      return randomFactors;
    },
    searchFactors() {
      console.log('搜索设置项:', this.searchName, this.searchCode);
    },
    resetSearch() {
      this.searchName = '';
      this.searchCode = '';
    },
    openAddDialog() {
      this.isEdit = false;
      this.currentFactor = { settingItem: '', settingContent: '' };
      this.dialogVisible = true;
    },
    editFactor(factor) {
      this.isEdit = true;
      this.editIndex = this.factors.indexOf(factor);
      this.currentFactor = { ...factor };
      this.dialogVisible = true;
    },
    saveFactor() {
      if (this.isEdit && this.editIndex !== null) {
        // 编辑模式，更新对应的记录
        this.$set(this.factors, this.editIndex, { ...this.currentFactor });
      } else {
        // 新增模式，添加新记录
        this.factors.push({ ...this.currentFactor });
      }
      this.dialogVisible = false;
    },
    cancelDialog() {
      this.dialogVisible = false;
    },
    deleteFactor(factor) {
      this.factors = this.factors.filter((f) => f !== factor);
    },
  },
};
</script>

<style scoped>
.emission-factor-manager {
  padding: 20px;
}

.search-bar {
  margin-bottom: 10px;
}

.table-container {
  width: 100%;
  overflow-x: auto;
}
</style>
