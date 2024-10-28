<template>
  <div id="app" style="width: 100%;display: flex">
    <div class="sidebar">
      <h3>模型管理</h3>
      <div class="button-group">
        <el-button type="primary" @click="currentView = 'createModel'">新建模型</el-button>
        <el-button type="info" @click="currentView = 'editModel'">编辑模型</el-button>
        <el-button type="success" @click="currentView = 'viewModel'">查看模型</el-button>
        <el-button type="danger" @click="currentView = 'deleteModel'">删除模型</el-button>
      </div>
    </div>
    <div class="content">
      <div v-if="currentView === 'createModel'">
        <el-image src="/images/smzqmxgl.png"></el-image>
      </div>
      <div v-if="currentView === 'editModel'">
        <el-image src="/images/smzqmxgl.png"></el-image>
      </div>
      <div v-if="currentView === 'viewModel'">
        <el-image src="/images/smzqmxgl.png"></el-image>
      </div>
      <div v-if="currentView === 'deleteModel'">
        <el-image src="/images/smzqmxgl.png"></el-image>
      </div>
    </div>
  </div>
</template>

<script>
  export default {
    data() {
      return {
        currentView: 'createModel' // 默认显示的新建模型内容
      };
    }
  };
</script>

<style scoped>
  .main-container {

  }

  .sidebar {
    width: 250px;
    border-right: 2px solid #eaeaea;
    padding: 20px;
    background-color: #f5f5f5;
    box-shadow: 2px 0 5px rgba(0, 0, 0, 0.1);
  }

  .sidebar h3 {
    margin-bottom: 20px;
    text-align: center; /* 居中标题 */
  }

  .button-group {
    display: flex;
    flex-direction: column; /* 竖直排列 */
  }

  .el-button {
    margin: 10px 0; /* 每个按钮间隔 */
    width: 100%; /* 按钮等宽填满 */
  }

  .content {
    flex: 1;
    padding: 20px;
    height: 100vh;
  }
</style>
