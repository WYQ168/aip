<template>
  <div class="carbon-dashboard">
    <!-- 碳资产价值展示 -->
    <div class="asset-value-container">
      <el-row :gutter="20">
        <el-col :span="12">
          <el-card class="value-card">
            <p>当前持有碳资产 (tCO2)</p>
            <b class="value-highlight">12,134,267</b>
          </el-card>
        </el-col>
        <el-col :span="12">
          <el-card class="value-card">
            <p>当前碳资产价值 (万元)</p>
            <b class="value-highlight">110.978</b>
          </el-card>
        </el-col>
      </el-row>
    </div>
    <br/>

    <div>
      <h3>持有资产表</h3>
      <el-table :data="tableData" style="width: 100%">
        <el-table-column prop="serialNumber" label="序号" ></el-table-column>
        <el-table-column prop="assetName" label="碳资产名称" ></el-table-column>
        <el-table-column prop="projectCode" label="项目编号" ></el-table-column>
        <el-table-column prop="entryPeriod" label="计入期" ></el-table-column>
        <el-table-column prop="holdings" label="持有量" ></el-table-column>
        <el-table-column prop="unit" label="单位" ></el-table-column>
        <el-table-column prop="description" label="说明" ></el-table-column>
        <el-table-column prop="price" label="单价" ></el-table-column>
        <el-table-column prop="assetScale" label="持有资产规模（万元）" ></el-table-column>
      </el-table>

      <p style="text-align: right;">单价取相关产品在对应市场近3个月成交价平均值，随市场变动。</p>
    </div>
    <br/>

    <div>
      <h3>成本表</h3>
      <el-table :data="tableData2" style="width: 100%; margin-bottom: 20px;"  :span-method="mergeCells">
        <el-table-column prop="actualEmissions" label="实际排放 (tCO2)" ></el-table-column>
        <el-table-column prop="allocation" label="分配配额 (tCO2)" ></el-table-column>
        <el-table-column prop="allocation" label="缺口 (tCO2)" ></el-table-column>
        <el-table-column prop="shortfall" label="购入CCER成本" >
          <el-table-column prop="purchaseAmountCCER" label="购入量 (tCO2)" ></el-table-column>
          <el-table-column prop="unitPriceCCER" label="单价 (元/tCO2)" ></el-table-column>
          <el-table-column prop="costCCER" label="成本 (元)" ></el-table-column>
        </el-table-column>
        <el-table-column prop="purchaseCEA" label="购入CEA成本">
          <el-table-column prop="purchaseAmountCEA" label="购入量 (tCO2)" ></el-table-column>
          <el-table-column prop="unitPriceCEA" label="单价 (元/tCO2)" ></el-table-column>
          <el-table-column prop="costCEA" label="成本 (元)" ></el-table-column>
        </el-table-column>
      </el-table>

      <p style="text-align: right;">注：根据国家相关规定，CCER购入量最大不得超过配额的5%</p>
      <p style="text-align: right;">CCER、CEA单价取自其在对应市场近3个月成交价平均值，随市场变动。</p>
    </div>

  </div>

</template>


<script>
import * as echarts from "echarts";

export default {
  data() {
    return {
      selectedYear: "2024年",
      selectedMarket: "national",
      years: ["2022年", "2023年", "2024年"],
      chartInstance: null,
      yearData: {
        "2022年": {
          排放量: [3000, 2000, 2500, 2700, 3200, 3400],
          配额量: [2500, 1800, 2000, 2400, 3000, 3300],
          深圳: [2200, 2300, 2000, 2600, 2800, 3000],
        },
        "2023年": {
          排放量: [4000, 3000, 3500, 3700, 3900, 4100],
          配额量: [3000, 2500, 2700, 2900, 3300, 3700],
          深圳: [2500, 2700, 2900, 3000, 3200, 3500],
        },
        "2024年": {
          排放量: [5000, 4000, 4500, 4700, 4900, 5100],
          配额量: [4000, 3500, 3700, 3900, 4300, 4500],
          深圳: [3000, 3200, 3400, 3500, 3700, 4000],
        },
      },
      tableData: [
        {
          serialNumber: 1,
          assetName: 'CCER开发',
          projectCode: 'HZJP00001',
          entryPeriod: '2020-2025',
          holdings: 23432,
          unit: 'tCO2',
          description: '企业持有',
          price: '68.13元/吨',
          assetScale: 159.64
        },
        {
          serialNumber: 2,
          assetName: 'CCER购入',
          projectCode: 'GRJPL0002',
          entryPeriod: '2019-2024',
          holdings: 10000,
          unit: 'tCO2',
          description: '企业购入',
          price: '58元/吨',
          assetScale: 58
        },
        {
          serialNumber: 3,
          assetName: 'VCS购入',
          projectCode: 'GRVCS0021',
          entryPeriod: '2018-2023',
          holdings: 20000,
          unit: 'tCO2',
          description: '企业购入',
          price: '25.32元/吨',
          assetScale: 50.64
        },
        {
          serialNumber: 4,
          assetName: '绿证购入',
          projectCode: 'GRLD0011',
          entryPeriod: '2024.1-2024.12',
          holdings: 200,
          unit: '张',
          description: '企业购入',
          price: '10元/张',
          assetScale: 0.2
        }
      ],
      tableData2: [
        {
          actualEmissions: 200000,
          allocation: 180000,
          shortfall: 20000,
          purchaseAmountCCER: 9000,
          unitPriceCCER: 63,
          costCCER: 567000,
          purchaseAmountCEA: '',
          unitPriceCEA: '',
          costCEA: ''
        }
      ]

    };
  },
  mounted() {
    this.initTrendChart();
  },
  methods: {
    initTrendChart() {
      const chartDom = this.$refs.trendChart;
      this.chartInstance = echarts.init(chartDom);

      this.updateChart(); // 初始化时加载默认年份数据
    },
    mergeCells({ row, column, rowIndex, columnIndex }) {
      if (columnIndex === 0 || columnIndex === 1 || columnIndex === 2) {
        return {
          rowspan: 2,
          colspan: 1
        };
      }
      return {
        rowspan: 1,
        colspan: 1
      };
    },
    updateChart() {
      const year = this.selectedYear;
      const data = this.yearData[year];

      const option = {
        tooltip: { trigger: "axis" },
        legend: { data: ["排放量", "配额量", "Shenzhen"] },
        xAxis: {
          type: "category",
          data: ["1月", "3月", "5月", "7月", "9月", "11月"],
        },
        yAxis: { type: "value" },
        series: [
          {
            name: "排放量",
            type: "line",
            data: data.排放量,
            itemStyle: { color: "#8a2be2" },
          },
          {
            name: "配额量",
            type: "line",
            data: data.配额量,
            itemStyle: { color: "#ff6347" },
          },
          {
            name: "Shenzhen",
            type: "line",
            data: data.深圳,
            itemStyle: { color: "#00ced1" },
          },
        ],
      };

      this.chartInstance.setOption(option);
    },
  },
};
</script>

<style scoped>
.carbon-dashboard {
  padding: 20px;
}

.asset-value-container {
  margin-bottom: 20px;
}

.value-card {
  text-align: center;
  padding: 20px;
  background-color: #fff;
  border-radius: 8px;
  box-shadow: 0 2px 8px rgba(0, 0, 0, 0.1);
}

.value-highlight {
  font-size: 28px;
  color: #f5222d;
}


</style>
