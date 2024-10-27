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
        <el-table-column prop="settingItem" label="设置项" />
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
      searchName: '',
      searchCode: '',
      factors: this.generateRandomData(),
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
        '数据来源', '数据代表性', '数据格式', '数据单位',
        '有效数字', '数据频率', '数据校准'
      ];
      const settingContents = [
        '自动采集/手动输入', '代表工序/车间/全厂',
        '吨（t）/立方米（m³）/兆瓦时（MWh）/吉焦（GJ）',
        '保留小数点后几位', '数据输入频率（秒、日、月）',
        '数据格式的有效阈值'
      ];
      const randomFactors = Array.from({ length: 10 }, () => ({
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
