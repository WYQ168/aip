<template>
  <div class="process-flow-page">

    <div class="flow-chart">
      <div class="node-container" v-for="(node, index) in nodes" :key="index" :style="node.style" @click="handleNodeClick(node)">
        <div class="node" :class="node.isPrimary ? 'primary-node' : 'secondary-node'">
          <img :src="node.image" alt="node.name" class="node-image" />
          <div class="node-label">{{ node.name }}</div>
        </div>
      </div>
      <svg class="flow-lines">
        <path v-for="(line, index) in lines" :key="index" :d="line.d" stroke="#666" stroke-width="3" fill="none" marker-end="url(#arrow)" />
        <defs>
          <marker id="arrow" markerWidth="10" markerHeight="10" refX="8" refY="3" orient="auto" markerUnits="strokeWidth">
            <path d="M0,0 L0,6 L9,3 z" fill="#666" />
          </marker>
        </defs>
      </svg>
    </div>

    <el-dialog :visible.sync="editDialogVisible" title="编辑数据">
      <el-form :model="editFormData" label-width="100px" label-position="left" :inline="false">
        <el-form-item label="名称" label-width="100px">
          <el-input v-model="editFormData.name" style="flex: 1;width: 220px" ></el-input>
        </el-form-item>
        <el-form-item label="性质" label-width="100px">
          <el-select v-model="editFormData.property" placeholder="请选择性质">
            <el-option label="物质" value="物质"></el-option>
            <el-option label="能源" value="能源"></el-option>
            <el-option label="废弃物" value="废弃物"></el-option>
          </el-select>
        </el-form-item>
        <el-form-item label="代表性" label-width="100px">
          <el-select v-model="editFormData.daibiaoxing" placeholder="请选择代表性">
            <el-option label="企业水平" value="企业水平"></el-option>
            <el-option label="地区平均水平" value="地区平均水平"></el-option>
            <el-option label="行业平均水平" value="行业平均水平"></el-option>
            <el-option label="国家平均水平" value="国家平均水平"></el-option>
            <el-option label="全球平均水平" value="全球平均水平"></el-option>
          </el-select>
        </el-form-item>
        <el-form-item label="统计周期" label-width="100px">
          <el-select v-model="editFormData.cycle" placeholder="请选择统计周期">
            <el-option label="年度" value="年度"></el-option>
            <el-option label="季度" value="季度"></el-option>
            <el-option label="月度" value="月度"></el-option>
          </el-select>
        </el-form-item>
        <el-form-item label="数量" label-width="100px">
          <el-input v-model="editFormData.amount" style="flex: 1;width: 220px"></el-input>
        </el-form-item>
        <el-form-item label="关联数据" label-width="100px">
          <el-input v-model="editFormData.relatedData" style="flex: 1;width: 220px"></el-input>
        </el-form-item>
      </el-form>
      <div slot="footer" class="dialog-footer">
        <el-button @click="editDialogVisible = false">取消</el-button>
        <el-button type="primary" @click="saveEdit">保存</el-button>
      </div>
    </el-dialog>

    <div class="footer-buttons">
      <el-button type="primary" @click="handleSave" style="background-color: blue; color: white;">保存</el-button>
      <el-button type="default" @click="handleCancel" style="background-color: white; color: black;">取消</el-button>
    </div>
  </div>
</template>

<script>
import {MIN_INPUT_WIDTH} from "@riophae/vue-treeselect/src/constants";

export default {
  data() {
    return {
      searchQuery: "",
      editDialogVisible: false,
      editFormData: {
        name: "",
        property: "",
        daibiaoxing: "",
        cycle: "",
        amount: "",
        relatedData: "",
      },
      nodes: [
        { name: "铁矿石", isPrimary: false, style: { top: "50px", left: "50px"}, image: require('@/assets/images/铁矿石.png') },
        { name: "烧结", isPrimary: false, style: { top: "50px", left: "300px" }, image: require('@/assets/images/烧结.png') },
        { name: "高炉炼铁", isPrimary: false, style: { top: "100px", left: "500px" }, image: require('@/assets/images/高炉炼铁.png') },
        { name: "煤", isPrimary: false, style: { top: "180px", left: "50px" }, image: require('@/assets/images/煤.png') },
        { name: "炼焦", isPrimary: false, style: { top: "180px", left: "300px" }, image: require('@/assets/images/炼焦.png') },
        { name: "电炉炼钢", isPrimary: false, style: { top: "50px", left: "700px" }, image: require('@/assets/images/电炉炼钢.png') },
        { name: "转炉炼钢", isPrimary: false, style: { top: "180px", left: "700px" }, image: require('@/assets/images/转炉炼钢.png') },
        { name: "钢产品", isPrimary: false, style: { top: "350px", left: "50px" }, image: require('@/assets/images/钢产品.png') },
        { name: "延压加工", isPrimary: false, style: { top: "350px", left: "300PX" }, image: require('@/assets/images/延压加工.png') },
        { name: "连铸", isPrimary: false, style: { top: "350px", left: "500px" }, image: require('@/assets/images/连铸.png') },
        { name: "精炼", isPrimary: false, style: { top: "350px", left: "700px" }, image: require('@/assets/images/精炼.png') }
      ],
      lines: [
        { d: "M150,100 L300,100" }, // 铁矿石 -> 烧结
        { d: "M150,225 L300,225" }, // 煤 -> 炼焦
        { d: "M400,100 L500,150" }, // 烧结 -> 高炉炼铁
        { d: "M400,200 L500,150" }, // 炼焦 -> 高炉炼铁
        { d: "M600,150 L700,100" }, // 高炉炼铁 -> 电炉炼钢
        { d: "M600,150 L700,225" }, // 高炉炼铁 -> 转炉炼钢
        { d: "M800,125 C850,250 900,350 800,400" }, // 电炉炼钢 -> 精炼
        { d: "M800,225 C850,260 900,355 800,400" }, // 转炉炼钢 -> 精炼
        {d: "M700,400 L600,400"}, // 精炼 -> 连铸
        {d: "M500,400 L400,400"}, // 精炼 -> 连铸
        {d: "M500,400 L400,400"}, // 连铸 -> 延压加工
        {d: "M300,400 L150,400"}, // 延压加工 -> 钢产品
      ],
    };
  },
  methods: {
    MIN_INPUT_WIDTH() {
      return MIN_INPUT_WIDTH
    },
    handleNodeClick(node) {
      if (node.isPrimary || node.name === "精炼" || node.name === "铁矿石" || node.name === "煤") {
        this.editFormData = {
          name: node.name,
          property: "",
          daibiaoxing: "",
          cycle: "",
          amount: "",
          relatedData: "",
        };
        this.editDialogVisible = true;
      }
    },
    saveEdit() {
      console.log("保存编辑数据", this.editFormData);
      this.editDialogVisible = false;
    },
  },
};
</script>

<style scoped>
.process-flow-page {
  padding: 20px;
}

.header {
  display: flex;
  justify-content: space-between;
  align-items: center;
  margin-bottom: 20px;
}

.flow-chart {
  position: relative;
  height: 700px;
}

.node-container {
  position: absolute;
  cursor: pointer;
  text-align: center;
  white-space: nowrap;
}

.node {
  display: inline-block;
  padding: 10px;
  border-radius: 5px;
  background-color: #f9f9f9;
  box-shadow: 0 0 15px rgba(0, 0, 0, 0.15);
  transition: transform 0.3s;
}

.node:hover {
  transform: scale(1.05);
}

.node-image {
  width: 80px;
  height: 60px;
  display: block;
  margin: 0 auto;
}

.node-label {
  margin-top: 5px;
  font-size: 16px;
  color: #333;
}

.primary-node {
  border: 3px solid #007bff;
  background-color: #e6f2ff;
}

.secondary-node {
  border: 2px solid #ddd;
}

.flow-lines {
  position: absolute;
  top: 0;
  left: 0;
  width: 100%;
  height: 100%;
  pointer-events: none;
}

.dialog-footer {
  text-align: right;
}

.footer-buttons {
  position: fixed;
  bottom: 10%;
  right: 10%;
  display: flex;
  gap: 10px;
  justify-content: flex-end;
  width: 100%;
}

</style>
