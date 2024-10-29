<template>
  <div class="emission-factor-manager">
    <!-- 搜索栏 -->
    <el-row :gutter="20" class="search-bar">
      <el-col :span="8">
        <el-input v-model="searchSettingItem" placeholder="按设置项搜索" clearable />
      </el-col>
      <el-col :span="8">
        <el-input v-model="searchSettingContent" placeholder="按设置内容搜索" clearable />
      </el-col>
      <el-col :span="8" class="search-buttons">
        <el-button type="primary" @click="searchFactors">搜索</el-button>
        <el-button @click="resetSearch">重置</el-button>
      </el-col>
    </el-row>

    <!-- 新增按钮 -->
    <el-button type="success" icon="el-icon-plus" @click="openAddDialog" style="margin: 10px 0;">
      新增设置项
    </el-button>

    <!-- 表格容器 -->
    <div class="table-container">
      <el-table :data="filteredFactors" border style="width: 100%;">
        <el-table-column type="selection" width="55"></el-table-column>
        <el-table-column prop="settingItem" label="排放因子名称" />
        <el-table-column prop="settingContent" label="数值" />
        <el-table-column prop="unit" label="单位" />
        <el-table-column label="操作">
          <template #default="scope">
            <el-button type="text" @click="editFactor(scope.$index, scope.row)">编辑</el-button>
            <el-button type="text" style="color: red;" @click="deleteFactor(scope.$index)">
              删除
            </el-button>
          </template>
        </el-table-column>
      </el-table>
    </div>

    <!-- 新增/编辑对话框 -->
    <el-dialog :title="isEdit ? '编辑设置项' : '新增设置项'" :visible.sync="dialogVisible">
      <el-form :model="currentFactor" label-width="120px">
        <el-form-item label="设置项" required>
          <el-input v-model="currentFactor.settingItem" />
        </el-form-item>
        <el-form-item label="设置内容" required>
          <el-input v-model="currentFactor.settingContent" />
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
      searchSettingItem: '',
      searchSettingContent: '',
      factors: this.generateRandomData(), // 生成随机数据
      dialogVisible: false,
      isEdit: false,
      editIndex: null,
      currentFactor: {
        settingItem: '',
        settingContent: '',
      },
    };
  },
  computed: {
    filteredFactors() {
      return this.factors.filter(
        (factor) =>
          (!this.searchSettingItem || factor.settingItem.includes(this.searchSettingItem)) &&
          (!this.searchSettingContent || factor.settingContent.includes(this.searchSettingContent))
      );
    },
    isFormValid() {
      return this.currentFactor.settingItem && this.currentFactor.settingContent;
    },
  },
  methods: {
    generateRandomData() {
      const settingItems = ['汽油', '天然气', '煤气', '柴油'];
      const units = ['tCO2/t', 'tCO2/10^4Nm3', 'tCO2/t'];
      const settingContents = [
        '2.925',
        '2.162',
        '1.900',
        '1.850',
      ];

      return Array.from({ length: 10 }, () => ({
        settingItem: settingItems[Math.floor(Math.random() * settingItems.length)],
        unit: units[Math.floor(Math.random() * units.length)],
        settingContent: settingContents[Math.floor(Math.random() * settingContents.length)],
      }));
    },
    searchFactors() {
      console.log('搜索设置项:', this.searchSettingItem, this.searchSettingContent);
    },
    resetSearch() {
      this.searchSettingItem = '';
      this.searchSettingContent = '';
    },
    openAddDialog() {
      this.isEdit = false;
      this.currentFactor = { settingItem: '', settingContent: '' };
      this.dialogVisible = true;
    },
    editFactor(index, factor) {
      this.isEdit = true;
      this.editIndex = index;
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
    deleteFactor(index) {
      this.factors.splice(index, 1);
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
