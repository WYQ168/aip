<template>
  <div class="carbon-asset-dashboard">
    <h2>碳资产构成</h2>

    <!-- 扇形环形图 -->
    <el-card class="chart-card">
      <div ref="pieChart" style="height: 300px;"></div>
    </el-card>

    <!-- 操作栏 -->
    <div class="table-controls">
      <el-button type="primary" icon="el-icon-plus" @click="openDialog('add')">新增记录</el-button>
      <el-button icon="el-icon-search" plain>筛选</el-button>
    </div>

    <!-- 碳资产表格 -->
    <el-table :data="tableData" border style="width: 100%; margin-top: 20px;">
      <el-table-column prop="type" label="碳资产类型" />
      <el-table-column prop="quantity" label="数据" />
      <el-table-column prop="remark" label="项目编号" />
      <el-table-column label="操作" width="150">
        <template #default="scope">
          <el-button type="text" icon="el-icon-edit" @click="openDialog('edit', scope.$index)"></el-button>
          <el-button type="text" icon="el-icon-delete" @click="deleteRecord(scope.$index)"></el-button>
        </template>
      </el-table-column>
    </el-table>

    <!-- 编辑/新增对话框 -->
    <el-dialog :title="dialogTitle" :visible.sync="dialogVisible" width="500px">
      <el-form :model="currentRecord" label-width="120px">
        <el-form-item label="碳资产类型">
          <el-input v-model="currentRecord.type" placeholder="请输入碳资产类型"/>
        </el-form-item>
        <el-form-item label="来源">
          <el-input v-model="currentRecord.source" placeholder="请输入来源"/>
        </el-form-item>
        <el-form-item label="数据量">
          <el-input v-model="currentRecord.quantity" placeholder="请输入数据量" type="number"/>
        </el-form-item>
        <el-form-item label="备注">
          <el-input v-model="currentRecord.remark" placeholder="请输入备注"/>
        </el-form-item>
      </el-form>
      <span slot="footer" class="dialog-footer">
        <el-button @click="dialogVisible = false">取消</el-button>
        <el-button type="primary" @click="saveRecord">保存</el-button>
      </span>
    </el-dialog>
  </div>
</template>

<script>
import * as echarts from "echarts";

export default {
  data() {
    return {
      dialogVisible: false,
      dialogTitle: "",
      isEditing: false,
      editIndex: -1,
      currentRecord: {type: "", source: "", quantity: "", remark: ""},
      tableData: [
        {type: "碳配额 (CEA)", source: "省生态厅发放", quantity: 546, remark: "总量配额"},
        {type: "中国核证自愿减排量 CCER", source: "CCER 市场购买", quantity: 457, remark: "CCER购买量"},
        {type: "国际核证自愿减排量 VCS", source: "VERRA 认证", quantity: 386, remark: "VCS购买量"},
        {type: "核证绿电购买量", source: "绿证市场购买", quantity: 64, remark: "绿电购买"},
      ],
    };
  },
  mounted() {
    this.initPieChart();
  },
  methods: {
    initPieChart() {
      const chartDom = this.$refs.pieChart;
      const myChart = echarts.init(chartDom);

      const option = {
        title: {
          text: "碳资产占比",
          left: "center",
          textStyle: {color: "#333", fontSize: 16},
        },
        tooltip: {trigger: "item"},
        legend: {
          orient: "vertical",
          left: "right",
          data: ["CEA", "CCER", "VCS", "绿电"],
        },
        series: [
          {
            name: "碳资产占比",
            type: "pie",
            radius: ["50%", "70%"], // 环形饼图
            avoidLabelOverlap: false,
            label: {show: false, position: "center"},
            emphasis: {
              label: {
                show: true,
                fontSize: "18",
                fontWeight: "bold",
              },
            },
            labelLine: {show: false},
            data: [
              {value: 546, name: "CEA"},
              {value: 457, name: "CCER"},
              {value: 386, name: "VCS"},
              {value: 64, name: "绿电"},
            ],
          },
        ],
      };

      myChart.setOption(option);
    },
    openDialog(action, index = -1) {
      this.dialogVisible = true;
      if (action === "edit") {
        this.dialogTitle = "编辑记录";
        this.isEditing = true;
        this.editIndex = index;
        this.currentRecord = {...this.tableData[index]};
      } else {
        this.dialogTitle = "新增记录";
        this.isEditing = false;
        this.currentRecord = {type: "", source: "", quantity: "", remark: ""};
      }
    },
    saveRecord() {
      if (this.isEditing) {
        this.$set(this.tableData, this.editIndex, {...this.currentRecord});
      } else {
        this.tableData.push({...this.currentRecord});
      }
      this.dialogVisible = false;
    },
    deleteRecord(index) {
      this.tableData.splice(index, 1);
    },
  },
};
</script>

<style scoped>
.carbon-asset-dashboard {
  padding: 20px;
}

.chart-card {
  margin-bottom: 20px;
}

.table-controls {
  display: flex;
  justify-content: space-between;
  margin-bottom: 10px;
}

.el-button {
  margin-left: 10px;
}

.dialog-footer {
  text-align: right;
}
</style>
