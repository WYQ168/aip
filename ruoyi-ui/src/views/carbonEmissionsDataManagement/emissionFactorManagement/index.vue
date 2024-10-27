<template>
  <div class="emission-factor-manager">
    <!-- 搜索栏 -->
    <el-row :gutter="20" class="search-bar">
      <el-col :span="8">
        <el-input v-model="searchName" placeholder="按名称搜索" clearable />
      </el-col>
      <el-col :span="8">
        <el-input v-model="searchCode" placeholder="按因子编码搜索" clearable />
      </el-col>
      <el-col :span="8" class="search-buttons">
        <el-button type="primary" @click="searchFactors">搜索</el-button>
        <el-button @click="resetSearch">重置</el-button>
      </el-col>
    </el-row>

    <!-- 新增按钮 -->
    <el-button type="success" icon="el-icon-plus" @click="openAddDialog" style="margin: 10px 0;">
      新增排放因子
    </el-button>

    <!-- 表格容器 -->
    <div class="table-container">
      <el-table :data="filteredFactors" border style="width: 100%;">
        <el-table-column type="selection" width="55"></el-table-column>
        <el-table-column prop="name" label="名称" />
        <el-table-column prop="code" label="因子编码" />
        <el-table-column prop="co2Value" label="二氧化碳排放因子值" />
        <el-table-column prop="unit" label="单位"/>
        <el-table-column prop="category" label="种类" />
        <el-table-column prop="property" label="性质" />
        <el-table-column label="操作">
          <template #default="scope">
            <el-button type="text" @click="editFactor(scope.row)">编辑</el-button>
            <el-button type="text" @click="deleteFactor(scope.row)" style="color: red;">删除</el-button>
          </template>
        </el-table-column>
      </el-table>
    </div>

    <!-- 新增/编辑排放因子对话框 -->
    <el-dialog :title="isEdit ? '编辑排放因子' : '新增排放因子'" :visible.sync="dialogVisible">
      <el-form :model="currentFactor" label-width="120px">
        <el-form-item label="名称">
          <el-input v-model="currentFactor.name" />
        </el-form-item>
        <el-form-item label="因子编码">
          <el-input v-model="currentFactor.code" />
        </el-form-item>
        <el-form-item label="二氧化碳排放因子值">
          <el-input v-model="currentFactor.co2Value" type="number" />
        </el-form-item>
        <el-form-item label="单位">
          <el-select v-model="currentFactor.unit" placeholder="请选择单位">
            <el-option label="tCO₂/t" value="tCO₂/t" />
          </el-select>
        </el-form-item>
        <el-form-item label="种类">
          <el-select v-model="currentFactor.category" placeholder="请选择种类">
            <el-option label="工业过程排放因子" value="工业过程排放因子" />
            <el-option label="其他排放因子" value="其他排放因子" />
          </el-select>
        </el-form-item>
        <el-form-item label="性质">
          <el-input v-model="currentFactor.property" />
        </el-form-item>
      </el-form>
      <template #footer>
        <el-button @click="dialogVisible = false">取消</el-button>
        <el-button type="primary" @click="saveFactor">保存</el-button>
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
      factors: [
        {name: '石灰石', code: 'SHS', co2Value: 0.44, unit: 'tCO₂/t', category: '工业过程排放因子', property: '熔剂'},
        {name: '白云石', code: 'BYS', co2Value: 0.471, unit: 'tCO₂/t', category: '工业过程排放因子', property: '熔剂'},
        {name: '电极', code: 'DJ', co2Value: 3.663, unit: 'tCO₂/t', category: '工业过程排放因子', property: '电极'},
      ],
      dialogVisible: false,
      isEdit: false,
      currentFactor: {
        name: '',
        code: '',
        co2Value: null,
        unit: 'tCO₂/t',
        category: '',
        property: '',
      },
    };
  },
  computed: {
    filteredFactors() {
      return this.factors.filter(
        (factor) =>
          (!this.searchName || factor.name.includes(this.searchName)) &&
          (!this.searchCode || factor.code.includes(this.searchCode))
      );
    },
  },
  methods: {
    searchFactors() {
      console.log('搜索因子:', this.searchName, this.searchCode);
    },
    resetSearch() {
      this.searchName = '';
      this.searchCode = '';
    },
    openAddDialog() {
      this.isEdit = false;
      this.currentFactor = {
        name: '',
        code: '',
        co2Value: null,
        unit: 'tCO₂/t',
        category: '',
        property: '',
      };
      this.dialogVisible = true;
    },
    editFactor(factor) {
      this.isEdit = true;
      this.currentFactor = {...factor};
      this.dialogVisible = true;
    },
    saveFactor() {
      if (this.isEdit) {
        const index = this.factors.findIndex((f) => f.code === this.currentFactor.code);
        if (index !== -1) {
          this.factors.splice(index, 1, this.currentFactor);
        }
      } else {
        this.factors.push({...this.currentFactor});
      }
      this.dialogVisible = false;
    },
    deleteFactor(factor) {
      this.factors = this.factors.filter((f) => f.code !== factor.code);
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
