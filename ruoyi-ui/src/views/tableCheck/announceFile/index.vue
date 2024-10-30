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
        <el-button type="primary" @click="generateDisclosureFile">生成报告</el-button>
        <el-button type="primary" >下载报告</el-button>
      </el-col>
    </el-row>

    <el-card class="disclosure-content" v-if="showContent">
      <h3>披露文件内容</h3>
      <div class="content-section">
        <el-input type="textarea" v-model="disclosureContent" placeholder="请输入披露内容..." />
      </div>
    </el-card>

    <el-btn type="success" v-if="showContent" @click="downloadPDF" style="margin-top: 20px;">下载 PDF</el-btn>
    <h3 style="display: inline-block; margin-right: 20px;">排放总计量表-可修正</h3>
    <el-button type="primary" style="background-color: #1E90FF; margin-left: 10px;">保存</el-button>
    <el-button type="primary" style="background-color: #1E90FF; border: 10px ">清空</el-button>

    <el-table :data="tableData" style="width: 100%; margin-top: 20px;" :span-method="mergeCells">
      <el-table-column prop="category" label="" ></el-table-column>
      <el-table-column prop="value" label="数据值" ></el-table-column>
      <el-table-column prop="unit" label="单位" ></el-table-column>
      <el-table-column prop="adjustedValue" label="修正值">
        <template slot-scope="scope">
          <el-input v-model="scope.row.adjustedValue" size="small"/>
        </template>
      </el-table-column>
    </el-table>
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
        tableData: [
          { category: '排放量', value: '化石燃料燃烧排放', unit: 'tCO2eq', adjustedValue: '', amount: '9626162.28' },
          { category: '', value: '净外购使用电力排放', unit: 'tCO2eq', adjustedValue: '', amount: '655526.74' },
          { category: '', value: '净外购使用热力排放', unit: 'tCO2eq', adjustedValue: '', amount: '-77214.14' },
          { category: '', value: '固碳产品隐含排放', unit: 'tCO2eq', adjustedValue: '', amount: '75474.92' },
          { category: '', value: '生产过程排放', unit: 'tCO2eq', adjustedValue: '', amount: '637434.07' },
          { category: '核算边界内碳排放总量', value: '10917383.87',  style: { color: 'red' }, unit: 'tCO2eq', adjustedValue: '', isHighlighted: true },
          { category: '产品产量', value: '5292508.59', unit: 't', adjustedValue: '' },
          { category: '排放强度', value: '2.06',  style: { color: 'red' }, unit: 'tCO2/t', adjustedValue: '', isHighlighted: true },
          { category: '能源消耗量', value: '烟煤', unit: 'tce', adjustedValue: '', amount: '907000' },
          { category: '', value: '兰炭', unit: 'tce', adjustedValue: '', amount: '22440.52' },
          { category: '', value: '焦炭', unit: 'tce', adjustedValue: '', amount: '2206000' },
          { category: '', value: '天然气', unit: 'tce', adjustedValue: '', amount: '10657' },
          { category: '能源消耗总量', value: '......', unit: '', adjustedValue: '' }
        ]
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
