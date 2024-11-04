<template>
  <div class="report-page">
    <div class="header">
      <el-select v-model="selectedProduct" placeholder="选择产品" class="select-product" style="margin-right: 10px;">
        <el-option label="产品A" value="产品A"></el-option>
        <el-option label="产品B" value="产品B"></el-option>
        <el-option label="产品C" value="产品C"></el-option>
      </el-select>
    </div>

    <div class="actions">
    <el-button type="primary" @click="viewList" class="button-view-list" style="vertical-align: middle;">查看清单</el-button>
    </div>

    <div class="actions">
      <div class="export-section" style="margin-top: 1px;">
        <el-button type="primary" @click="exportReport" class="button-export">导出报告</el-button>
        <div style="margin-top: 1px;margin-left: 1px">
          <el-select v-model="selectedReport" placeholder="选择导出报告" class="select-report" style="margin-left: 10px">
            <el-option label="EPD报告" value="EPD报告"></el-option>
            <el-option label="产品碳足迹报告" value="产品碳足迹报告"></el-option>
            <el-option label="CBAM申报报告" value="CBAM申报报告"></el-option>
            <el-option label="其他报告" value="其他报告"></el-option>
          </el-select>
        </div>
      </div>
      <el-button type="primary" @click="viewReport" class="button-view-report" style="margin-left: 40px;">查看报告</el-button>
      <el-button type="primary" @click="editReport" class="button-edit-report" style="margin-left: 10px;">报告编辑</el-button>
    </div>

    <br/>
    <br/>
    <br/>
    <br/>
    <br/>
    <br/>
    <br/>
    <br/>
    <div class="footer">
      <el-button type="default" @click="viewHistory" class="button-view-history" style="margin-right: 10px;">历史报告查询</el-button>
      <el-input v-model="searchQuery" placeholder="搜索已导出报告" class="search-report" suffix-icon="el-icon-search"></el-input>
    </div>
  </div>
</template>

<script>
export default {
  data() {
    return {
      selectedProduct: "",
      selectedReport: "",
      searchQuery: "",
    };
  },
  methods: {
    viewList() {
      console.log("查看清单", this.selectedProduct);
    },
    exportReport() {
      console.log("导出报告", this.selectedReport);
    },
    viewReport() {
      console.log("查看报告");
    },
    editReport() {
      console.log("报告编辑");
    },
    viewHistory() {
      console.log("历史报告查询");
    },
  },
};
</script>

<style scoped>
.report-page {
  padding: 20px;
}

.header {
  display: flex;
  align-items: center;
  margin-bottom: 20px;
}

.select-product {
  margin-left: 20px;
  width: 200px;
}

.actions {
  display: flex;
  align-items: center;
  gap: 20px;
  margin-bottom: 20px;
}

.export-section {
  display: flex;
  align-items: center;
  gap: 10px;
}

.button-view-list,
.button-export,
.button-view-report,
.button-edit-report,
.button-view-history {
  background-color: #007bff;
  color: white;
}

.button-view-list {
  width: 120px;
}

.select-report {
  width: 200px;
}

.footer {
  display: flex;
  align-items: center;
  gap: 20px;
}

.search-report {
  width: 300px;
}
</style>
