<template>
  <div class="disclosure-file-generation">
    <el-header>
      <h2>碳排放管理系统 - 披露文件生成</h2>
    </el-header>

    <el-row class="filter-row" type="flex" justify="start">
      <el-col :span="12">
        <el-select v-model="selectedType" placeholder="选择披露类型">
          <el-option label="年度披露" value="annual"></el-option>
          <el-option label="月度披露" value="monthly"></el-option>
          <el-option label="项目披露" value="project"></el-option>
        </el-select>
      </el-col>
      <el-col :span="10">
        <el-button type="primary" @click="generateDisclosureFile">生成文件</el-button>
      </el-col>
    </el-row>

    <el-card class="disclosure-content" v-if="showContent">
      <h3>披露文件内容</h3>
      <div class="content-section">
        <el-input type="textarea" v-model="disclosureContent" placeholder="请输入披露内容..." />
      </div>
    </el-card>

    <el-btn type="success" v-if="showContent" @click="downloadPDF" style="margin-top: 20px;">下载 PDF</el-btn>
  </div>
</template>

<script>
  import jsPDF from 'jspdf';

  export default {
    data() {
      return {
        selectedType: '',
        showContent: false,
        disclosureContent: '',
      };
    },
    methods: {
      generateDisclosureFile() {
        if (!this.selectedType) {
          this.$message.warning('请选择披露类型');
          return;
        }

        // 这里可以根据选定的类型生成默认内容
        if (this.selectedType === 'annual') {
          this.disclosureContent = '年度碳排放披露内容：\n本年度碳排放总量为XXX吨...';
        } else if (this.selectedType === 'monthly') {
          this.disclosureContent = '本月碳排放披露内容：\n总排放量为XX吨...';
        } else if (this.selectedType === 'project') {
          this.disclosureContent = '项目碳排放披露内容：\n本项目共排放XX吨碳...';
        }

        this.showContent = true;
      },
      downloadPDF() {
        const doc = new jsPDF();
        doc.text('披露文件', 20, 20); // 添加标题
        doc.text('披露类型: ' + this.selectedType, 20, 30); // 披露类型
        doc.text('内容:', 20, 40); // 内容标题
        doc.text(this.disclosureContent.split('\n'), 20, 50); // 披露内容

        doc.save(`披露文件_${this.selectedType}.pdf`);
      },
    },
  };
</script>

<style scoped>
  .disclosure-file-generation {
    padding: 20px;
  }
  .filter-row {
    margin-bottom: 20px;
  }
  .disclosure-content {
    margin-top: 20px;
    padding: 15px;
  }
  .content-section {
    margin-top: 10px;
  }
</style>
