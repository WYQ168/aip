<template>
  <div class="activity-type-manager">
    <h2>活动数据类型管理</h2>

    <!-- 活动数据类型选择 -->
    <div class="form-group">
      <label for="activity-type-select">选择活动数据类型：</label>
      <select v-model="selectedType" id="activity-type-select" @change="handleTypeChange">
        <option disabled value="">请选择类型</option>
        <option v-for="type in activityTypes" :key="type" :value="type">{{ type }}</option>
      </select>
    </div>

    <!-- 设置详细数据 -->
    <div v-if="selectedType" class="type-details">
      <h3>当前选择的类型：{{ selectedType }}</h3>
      <label>数据来源：</label>
      <select v-model="dataSource">
        <option value="自动采集">自动采集</option>
        <option value="手动输入">手动输入</option>
      </select>

      <label>代表性：</label>
      <select v-model="representative">
        <option value="工序">工序</option>
        <option value="车间">车间</option>
        <option value="全厂">全厂</option>
      </select>

      <label>数据单位：</label>
      <select v-model="dataUnit">
        <option value="吨 (t)">吨 (t)</option>
        <option value="立方米 (m³)">立方米 (m³)</option>
        <option value="兆瓦时 (MWh)">兆瓦时 (MWh)</option>
        <option value="吉焦 (GJ)">吉焦 (GJ)</option>
      </select>

      <label>数据频率：</label>
      <select v-model="dataFrequency">
        <option value="秒">秒</option>
        <option value="时">时</option>
        <option value="日">日</option>
        <option value="周">周</option>
        <option value="月">月</option>
        <option value="季">季</option>
      </select>

      <button @click="saveSettings">保存设置</button>
    </div>

    <!-- 已保存的设置展示 -->
    <div v-if="savedSettings.length" class="saved-settings">
      <h3>已保存的设置</h3>
      <ul>
        <li v-for="(setting, index) in savedSettings" :key="index">
          <strong>类型：</strong> {{ setting.type }} |
          <strong>来源：</strong> {{ setting.source }} |
          <strong>代表性：</strong> {{ setting.representative }} |
          <strong>单位：</strong> {{ setting.unit }} |
          <strong>频率：</strong> {{ setting.frequency }}
        </li>
      </ul>
    </div>
  </div>
</template>

<script>
export default {
  name: 'ActivityTypeManager',
  data() {
    return {
      activityTypes: [
        '化石燃料',
        '过程原料',
        '外购电力',
        '自发电力',
        '外购热力',
        '自产热力',
        '固碳产品',
      ],
      selectedType: '',
      dataSource: '自动采集',
      representative: '工序',
      dataUnit: '吨 (t)',
      dataFrequency: '日',
      savedSettings: [],
    };
  },
  methods: {
    handleTypeChange() {
      console.log(`选择的活动数据类型：${this.selectedType}`);
    },
    saveSettings() {
      const newSetting = {
        type: this.selectedType,
        source: this.dataSource,
        representative: this.representative,
        unit: this.dataUnit,
        frequency: this.dataFrequency,
      };
      this.savedSettings.push(newSetting);
      this.resetForm();
    },
    resetForm() {
      this.selectedType = '';
      this.dataSource = '自动采集';
      this.representative = '工序';
      this.dataUnit = '吨 (t)';
      this.dataFrequency = '日';
    },
  },
};
</script>

<style scoped>
.activity-type-manager {
  margin: 20px;
}

.form-group {
  margin-bottom: 15px;
}

.type-details {
  margin-top: 20px;
}

button {
  margin-top: 15px;
  padding: 8px 16px;
  background-color: #42b983;
  color: white;
  border: none;
  cursor: pointer;
}

button:hover {
  background-color: #369b76;
}

.saved-settings ul {
  list-style-type: none;
  padding: 0;
}

.saved-settings li {
  margin: 10px 0;
  padding: 10px;
  background-color: #f0f0f0;
  border-radius: 5px;
}
</style>
