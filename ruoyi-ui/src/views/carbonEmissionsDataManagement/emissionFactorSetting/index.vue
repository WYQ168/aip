<template>
  <div class="emission-factor-manager">
    <!-- 搜索栏 -->
    <el-row :gutter="20" class="search-bar">
      <el-col :span="8">
        <el-input v-model="searchSettingItem" placeholder="按设置项搜索" clearable />
      </el-col>
      <el-col :span="8">
        <el-input v-model="searchSettingContent" placeholder="按设置内容搜索" clearable />
      </el-col>
      <el-col :span="8" class="search-buttons">
        <el-button type="primary" @click="searchFactors">搜索</el-button>
        <el-button @click="resetSearch">重置</el-button>
      </el-col>
    </el-row>

    <!-- 新增按钮 -->
    <el-button type="success" icon="el-icon-plus" @click="openAddDialog" style="margin: 10px 0;">
      新增设置项
    </el-button>

    <!-- 表格容器 -->
    <div class="table-container">
      <el-table :data="filteredFactors" border style="width: 100%;">
        <el-table-column type="selection" width="55"></el-table-column>
        <el-table-column prop="settingItem" label="排放因子名称" />
        <el-table-column prop="settingContent" label="数值" />
        <el-table-column prop="unit" label="单位" />
        <el-table-column label="操作">
          <template #default="scope">
            <el-button type="text" @click="editFactor(scope.$index, scope.row)">编辑</el-button>
            <el-button type="text" style="color: red;" @click="deleteFactor(scope.$index)">
              删除
            </el-button>
          </template>
        </el-table-column>
      </el-table>
    </div>

    <!-- 新增/编辑对话框 -->
    <el-dialog :title="isEdit ? '排放因子输入设置' : '新增排放因子'" :visible.sync="dialogVisible">
      <template>
        <el-form :model="form" label-width="120px">
          <el-row :gutter="20">
            <el-col :span="24">
              <el-form-item label="碳氧化率">
                <el-row :gutter="10" align="middle">
                  <el-col :span="3">
                    <el-switch v-model="form.carbonRate.isRealValue" />
                    <span>实测值</span>
                  </el-col>
                </el-row>
                <el-row :gutter="10">
                  <el-col :span="8">
                    <div>数据值</div>
                    <el-input v-model="form.carbonRate.value" placeholder="请输入数据值"  size="small"></el-input>
                  </el-col>
                  <el-col :span="4">
                    <div>单位</div>
                    <el-select v-model="form.carbonRate.unit" placeholder="请选择单位" size="small">
                      <el-option label="%" value="%"></el-option>
                    </el-select>
                  </el-col>
                </el-row>
                <el-row :gutter="10" style="margin-top: 10px;">
                  <el-col :span="8">
                    <div>有效数字</div>
                    <el-select v-model="form.carbonRate.significantFigures" placeholder="请选择有效数字" size="small">
                      <el-option label="下拉框" value="下拉框"></el-option>
                    </el-select>
                  </el-col>
                  <el-col :span="8">
                    <div>有效阈值</div>
                    <el-input v-model="form.carbonRate.rangeMin"   placeholder="最小值" size="small"></el-input>
                    -
                    <el-input v-model="form.carbonRate.rangeMax"  placeholder="最大值" size="small"></el-input>
                  </el-col>
                </el-row>
              </el-form-item>
            </el-col>

            <el-col :span="24">
              <el-form-item>
                <el-radio v-model="form.calculationMethod" label="计算方式一">计算方式一</el-radio>
              </el-form-item>
            </el-col>

            <el-col :span="24">
              <el-form-item label="低位发热量">
                <el-row :gutter="10" align="middle">
                  <el-col :span="3">
                    <el-switch v-model="form.lowerCalorificValue.isRealValue" />
                    <span>实测值</span>
                  </el-col>
                </el-row>
                <el-row :gutter="10">
                  <el-col :span="8">
                    <div>数据值</div>
                    <el-input v-model="form.lowerCalorificValue.value" placeholder="请输入数据值"   size="small"></el-input>
                  </el-col>
                  <el-col :span="4">
                    <div>单位</div>
                    <el-select v-model="form.lowerCalorificValue.unit" placeholder="请选择单位" size="small">
                      <el-option label="GJ/t (缺省值)" value="GJ/t"></el-option>
                    </el-select>
                  </el-col>
                </el-row>
                <el-row :gutter="10" style="margin-top: 10px;">
                  <el-col :span="8">
                    <div>有效数字</div>
                    <el-select v-model="form.lowerCalorificValue.significantFigures" placeholder="请选择有效数字" size="small">
                      <el-option label="4 (缺省值)" value="4"></el-option>
                    </el-select>
                  </el-col>
                  <el-col :span="8">
                    <div>有效阈值</div>
                    <el-input v-model="form.lowerCalorificValue.rangeMin"     placeholder="最小值" size="small"></el-input>
                    -
                    <el-input v-model="form.lowerCalorificValue.rangeMax"     placeholder="最大值" size="small"></el-input>
                  </el-col>
                </el-row>
              </el-form-item>
            </el-col>

            <el-col :span="24">
              <el-form-item label="单位热值含碳量">
                <el-row :gutter="10" align="middle">
                  <el-col :span="3">
                    <el-switch v-model="form.unitHeatValueCarbon.isRealValue" />
                    <span>实测值</span>
                  </el-col>
                </el-row>
                <el-row :gutter="10">
                  <el-col :span="8">
                    <div>数据值</div>
                    <el-input v-model="form.unitHeatValueCarbon.value"  placeholder="请输入数据值" size="small"></el-input>
                  </el-col>
                  <el-col :span="4">
                    <div>单位</div>
                    <el-select v-model="form.unitHeatValueCarbon.unit" placeholder="请选择单位" size="small">
                      <el-option label="默认为C/GJ" value="默认为C/GJ"></el-option>
                    </el-select>
                  </el-col>
                </el-row>
                <el-row :gutter="10" style="margin-top: 10px;">
                  <el-col :span="8">
                    <div>有效数字</div>
                    <el-select v-model="form.unitHeatValueCarbon.significantFigures" placeholder="请选择有效数字" size="small">
                      <el-option label="4 (默认)" value="4"></el-option>
                    </el-select>
                  </el-col>
                  <el-col :span="8">
                    <div>有效阈值</div>
                    <el-input v-model="form.unitHeatValueCarbon.rangeMin"   placeholder="最小值" size="small"></el-input>
                    -
                    <el-input v-model="form.unitHeatValueCarbon.rangeMax"   placeholder="最大值" size="small"></el-input>
                  </el-col>
                </el-row>
              </el-form-item>
            </el-col>

            <el-col :span="24">
              <el-form-item>
                <el-radio v-model="form.calculationMethod" label="计算方式二">计算方式二</el-radio>
              </el-form-item>
            </el-col>

            <el-col :span="24">
              <el-form-item label="收到基元素含碳量">
                <el-row :gutter="10" align="middle">
                  <el-col :span="3">
                    <el-switch v-model="form.receivedElementCarbon.isRealValue" />
                    <span>实测值</span>
                  </el-col>
                </el-row>
                <el-row :gutter="10">
                  <el-col :span="8">
                    <div>数据值</div>
                    <el-input v-model="form.receivedElementCarbon.value"  placeholder="请输入数据值" size="small"></el-input>
                  </el-col>
                  <el-col :span="4">
                    <div>单位</div>
                    <el-select v-model="form.receivedElementCarbon.unit" placeholder="请选择单位" size="small">
                      <el-option label="下拉框 (缺省值)" value="下拉框"></el-option>
                    </el-select>
                  </el-col>
                </el-row>
                <el-row :gutter="10" style="margin-top: 10px;">
                  <el-col :span="8">
                    <div>有效数字</div>
                    <el-select v-model="form.receivedElementCarbon.significantFigures" placeholder="请选择有效数字" size="small">
                      <el-option label="4 (缺省值)" value="4"></el-option>
                    </el-select>
                  </el-col>
                  <el-col :span="8">
                    <div>有效阈值</div>
                    <el-input v-model="form.receivedElementCarbon.rangeMin"   placeholder="最小值" size="small"></el-input>
                    -
                    <el-input v-model="form.receivedElementCarbon.rangeMax"   placeholder="最大值" size="small"></el-input>
                  </el-col>
                </el-row>
              </el-form-item>
            </el-col>
          </el-row>

        </el-form>
      </template>

      <template #footer>
        <el-button @click="cancelDialog">取消</el-button>
        <el-button type="primary" @click="saveFactor" :disabled="!isFormValid">保存</el-button>
      </template>
    </el-dialog>
  </div>
</template>

<style scoped>
.el-form-item {
  margin-bottom: 20px;
}
</style>
<script>
export default {
  data() {
    return {
      searchSettingItem: '',
      searchSettingContent: '',
      factors: this.generateRandomData(), // 生成随机数据
      dialogVisible: false,
      isEdit: false,
      editIndex: null,
      currentFactor: {
        settingItem: '',
        settingContent: '',
      },
      form: {
        carbonRate: {
          isRealValue: true,
          value: '',
          unit: '%',
          significantFigures: '',
          rangeMin: '',
          rangeMax: ''
        },
        calculationMethod: '计算方式一',
        lowerCalorificValue: {
          isRealValue: false,
          value: '',
          unit: 'GJ/t',
          significantFigures: '',
          rangeMin: '',
          rangeMax: ''
        },
        unitHeatValueCarbon: {
          isRealValue: true,
          value: '',
          unit: '默认为C/GJ',
          significantFigures: '',
          rangeMin: '',
          rangeMax: ''
        },
        receivedElementCarbon: {
          isRealValue: false,
          value: '',
          unit: '下拉框',
          significantFigures: '',
          rangeMin: '',
          rangeMax: ''
        }
      }
    };
  },
  computed: {
    filteredFactors() {
      return this.factors.filter(
        (factor) =>
          (!this.searchSettingItem || factor.settingItem.includes(this.searchSettingItem)) &&
          (!this.searchSettingContent || factor.settingContent.includes(this.searchSettingContent))
      );
    },
    isFormValid() {
      return this.currentFactor.settingItem && this.currentFactor.settingContent;
    },
  },
  methods: {
    generateRandomData() {
      const settingItems = ['汽油', '天然气', '煤气', '柴油'];
      const units = ['tCO2/t', 'tCO2/10^4Nm3', 'tCO2/t'];
      const settingContents = [
        '2.925',
        '2.162',
        '1.900',
        '1.850',
      ];

      return Array.from({ length: 10 }, () => ({
        settingItem: settingItems[Math.floor(Math.random() * settingItems.length)],
        unit: units[Math.floor(Math.random() * units.length)],
        settingContent: settingContents[Math.floor(Math.random() * settingContents.length)],
      }));
    },
    searchFactors() {
      console.log('搜索设置项:', this.searchSettingItem, this.searchSettingContent);
    },
    resetSearch() {
      this.searchSettingItem = '';
      this.searchSettingContent = '';
    },
    openAddDialog() {
      this.isEdit = false;
      this.currentFactor = { settingItem: '', settingContent: '' };
      this.dialogVisible = true;
    },
    editFactor(index, factor) {
      this.isEdit = true;
      this.editIndex = index;
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
    deleteFactor(index) {
      this.factors.splice(index, 1);
    },
    onSave() {
      console.log('保存数据', this.form);
    },
    onCancel() {
      console.log('取消操作');
    }
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
