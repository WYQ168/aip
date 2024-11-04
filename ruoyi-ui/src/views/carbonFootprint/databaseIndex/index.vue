<template>
  <div class="data-page">
    <el-card class="box-card">
      <div slot="header" class="clearfix">
        <el-button type="primary" @click="toggleDatabaseVisibility">
          {{ isDatabaseVisible ? '隐藏已导入数据库' : '已导入数据库' }}
        </el-button>
      </div>
      <div v-if="isDatabaseVisible" class="imported-database">
        <div v-for="(db, index) in importedDatabases" :key="index" class="database-item">
          {{ db }}
        </div>
      </div>
    </el-card>

    <br/>
    <div class="edit-database">
      <el-button type="primary" class="edit-button">编辑数据库</el-button>
      <el-select v-model="selectedDatabase" placeholder="选择数据库" class="database-select">
        <el-option
          v-for="(db, index) in importedDatabases"
          :key="index"
          :label="db"
          :value="db"
        />
      </el-select>
      <el-input v-model="searchQuery" placeholder="搜索对象" class="search-input"></el-input>
    </div>

    <el-table :data="filteredData" border style="width: 100%" class="data-table">
      <el-table-column prop="project" label="项目" :min-width="'150px'"></el-table-column>
      <el-table-column prop="value" label="数值" :min-width="'150px'"></el-table-column>
      <el-table-column prop="period" label="周期" :min-width="'150px'"></el-table-column>
      <el-table-column prop="representative" label="数据代表性" :min-width="'200px'"></el-table-column>
      <el-table-column label="操作" :min-width="'120px'">
        <template slot-scope="scope">
          <el-button type="text" icon="el-icon-edit" @click="handleEdit(scope.row)"></el-button>
          <el-button type="text" icon="el-icon-delete" @click="handleDelete(scope.row)"></el-button>
        </template>
      </el-table-column>
    </el-table>

    <el-dialog :visible.sync="editDialogVisible" title="编辑数据">
      <el-form :model="editFormData">
        <el-form-item label="项目">
          <el-input v-model="editFormData.project"></el-input>
        </el-form-item>
        <el-form-item label="数值">
          <el-input v-model="editFormData.value"></el-input>
        </el-form-item>
        <el-form-item label="周期">
          <el-input v-model="editFormData.period"></el-input>
        </el-form-item>
        <el-form-item label="数据代表性">
          <el-input v-model="editFormData.representative"></el-input>
        </el-form-item>
      </el-form>
      <div slot="footer" class="dialog-footer">
        <el-button @click="editDialogVisible = false">取消</el-button>
        <el-button type="primary" @click="saveEdit">保存</el-button>
      </div>
    </el-dialog>
  </div>
</template>

<script>
export default {
  data() {
    return {
      importedDatabases: [
        "企业数据库",
        "中国全生命周期数据库CLCD",
        "天工LCA数据库",
        "GABI数据库",
      ],
      selectedDatabase: "",
      searchQuery: "",
      isDatabaseVisible: false,
      editDialogVisible: false,
      editFormData: {
        project: "",
        value: "",
        period: "",
        representative: "",
      },
      tableData: [
        {
          project: "电力",
          value: "0.203kg/kWh",
          period: "从摇篮到坟墓",
          representative: "华北-火力发电-2021年",
        },
        {
          project: "水资源",
          value: "0.05m3/吨",
          period: "从摇篮到坟墓",
          representative: "华东-水资源使用-2021年",
        },
        {
          project: "天然气",
          value: "0.15kg/m3",
          period: "从摇篮到坟墓",
          representative: "西南-天然气开采-2020年",
        },
        {
          project: "钢材",
          value: "2.3kg/吨",
          period: "从摇篮到门",
          representative: "东北-钢材制造-2022年",
        },
      ],
    };
  },
  computed: {
    filteredData() {
      if (this.searchQuery.trim()) {
        return this.tableData.filter((item) =>
          item.project.includes(this.searchQuery.trim())
        );
      }
      return this.tableData;
    },
  },
  methods: {
    handleDatabaseClick(db) {
      console.log("Selected database:", db);
    },
    toggleDatabaseVisibility() {
      this.isDatabaseVisible = !this.isDatabaseVisible;
    },
    handleEdit(row) {
      this.editFormData = { ...row };
      this.editDialogVisible = true;
    },
    handleDelete(row) {
      this.tableData = this.tableData.filter(item => item !== row);
    },
    saveEdit() {
      const index = this.tableData.findIndex(item => item.project === this.editFormData.project);
      if (index !== -1) {
        this.tableData.splice(index, 1, { ...this.editFormData });
      }
      this.editDialogVisible = false;
    },
  },
};
</script>

<style scoped>
.data-page {
  padding: 20px;
}

.imported-database {
  margin-bottom: 20px;
}

.database-item {
  padding: 10px;
  border: 1px solid #ddd;
  margin: 5px 0;
  background-color: #f9f9f9;
}

.edit-database {
  display: flex;
  align-items: center;
  margin-bottom: 20px;
}

.edit-button {
  margin-right: 10px;
}

.database-select {
  margin-right: 10px;
}

.search-input {
  flex-grow: 1;
}

.data-table {
  margin-top: 20px;
}
</style>
