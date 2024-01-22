<template>
  <q-page padding>
    <div class="row justify-center q-gutter-md">
      <q-list class="col-md-5 custom-bg" bordered padding>
        <q-item>
        <q-item-section>Parameter</q-item-section>
        <q-item-section>Reference Interval</q-item-section>
        <q-item-section>Input Value</q-item-section>
        <q-item-section>Status</q-item-section>
      </q-item>

      <q-item v-for="(value, key) in job_store.originalFormValues" :key="key" class="row items-center">
        <q-item-section class="col-3">{{ key }}</q-item-section>
        <q-item-section class="col-3">-</q-item-section> <!-- 示例中的 Reference Interval 是 '-' -->
        <q-item-section class="col-3">{{ value }}</q-item-section>
        <q-item-section class="col-3">
          <!-- 在这里可以根据 value 的值来显示不同的状态 -->
          <div class="status-indicators">
            <div class="status-circle red" v-show="getStatusClass(value) === 'high'"></div>
            <div class="status-circle yellow" v-show="getStatusClass(value) === 'medium'"></div>
            <div class="status-circle green" v-show="getStatusClass(value) === 'low'"></div>
          </div>
        </q-item-section>
      </q-item>
        <!-- <q-item v-ripple>
          <q-item-section>
            <q-item-label header>Risk Score</q-item-label>
          </q-item-section>
          <q-item-section>
            {{job_store.risk_score}}
          </q-item-section>
        </q-item> -->

        <!-- <q-item v-ripple>
          <q-item-section>
            <q-item-label header>Risk Group</q-item-label>
          </q-item-section>
          <q-item-section>
            {{job_store.risk_group}}
          </q-item-section>
        </q-item>

        <q-item v-ripple>
          <q-item-section>
            <q-item-label header>2 year survival rate</q-item-label>
          </q-item-section>
          <q-item-section>
            {{job_store.get2yprob}}
          </q-item-section>
        </q-item>

        <q-item v-ripple>
          <q-item-section>
            <q-item-label header>Tumor size:</q-item-label>
          </q-item-section>
          <q-item-section>
            {{job_store.originalFormValues.tumor_size}}
          </q-item-section>
        </q-item> -->

        <!-- <q-item v-ripple>
          <q-item-section>
            <q-item-label header>Model advice</q-item-label>
          </q-item-section>
          <q-item-section>
          建議：<br>

          1. 健康飲食：請保持均衡的飲食，多吃蔬菜和水果，限制高糖、高鹽和高脂肪食物的攝入量。建議選擇全穀類食物，減少加工食品的消費。<br>

          2. 規律運動：建議每週至少進行150分鐘的中等強度有氧運動，如快走、游泳或騎自行車。運動可以幫助保持健康的體重，減少慢性疾病的風險。<br>

          3. 充足睡眠：每晚保證7-8小時的優質睡眠。良好的睡眠對身體和心理健康至關重要。<br>

          4. 減少壓力：盡量減少生活中的壓力。可以嘗試冥想、深呼吸練習或瑜伽來放鬆身心。<br>

          5. 定期體檢：建議每年進行一次全面的健康檢查，以及時發現和處理健康問題。<br>

          6. 戒煙限酒：若有吸煙或飲酒習慣，建議戒煙並限制酒精攝入，這對健康有顯著益處。<br>

          請根據自身情況，適當調整生活習慣，並在必要時諮詢專業醫生的意見。<br>

          </q-item-section>
        </q-item> -->

        
      </q-list>

      <q-card
        class="col-md-5 custom-bg"
        flat
        bordered
      >
        <q-card-section>
          <q-markup-table :separator="'vertical'" flat bordered>
            <thead>
              <tr>
                <th class="text-left">Monthss</th>
                <th class="text-right">6</th>
                <th class="text-right">12</th>
                <th class="text-right">18</th>
                <th class="text-right">24</th>
                <!-- <th class="text-right">5</th> --> 
              </tr>
            </thead>
            <tbody>
              <tr>
                <td class="text-left">Recurrence free survival rate</td>
                <td class="text-right">{{ job_store.survival_rates[0] }}</td>
                <td class="text-right">{{ job_store.survival_rates[1] }}</td>
                <td class="text-right">{{ job_store.survival_rates[2] }}</td>
                <td class="text-right">{{ job_store.survival_rates[3] }}</td>
                <!-- <td class="text-right">{{ job_store.survival_rates[4] }}</td> -->
              </tr>
            </tbody>
          </q-markup-table>
        </q-card-section>
        <q-card-section class="row justify-center">
          <Bar
            id="survival-chart"
            :options="chart_options"
            :data="survival_data"
          />
        </q-card-section>
        <q-card-section class="q-pt-none">
          <div class="text-subtitle2">Prediction Status</div>
          <div class="q-mt-md">
            <div class="flex items-center q-mb-xs">
              <div class="status-circle red"></div>
              <span>Status High: The clinical value is higher than the reference interval.</span>
            </div>
            <div class="flex items-center q-mb-xs">
              <div class="status-circle yellow"></div>
              <span>Status Medium: The clinical value is slightly higher than the reference interval.</span>
            </div>
            <div class="flex items-center">
              <div class="status-circle green"></div>
              <span>Status Normal: This clinical value is in the reference interval.</span>
            </div>
          </div>
      </q-card-section>
      </q-card>
    </div>

  </q-page>
</template>

<script>
import { Bar } from 'vue-chartjs'
import { Chart as ChartJS, Title, Tooltip, Legend, BarElement, CategoryScale, LinearScale } from 'chart.js'
import { useJobStore } from 'stores/job'

ChartJS.register(Title, Tooltip, Legend, BarElement, CategoryScale, LinearScale)

export default {
  name: 'CaseResultPage',
  components: { Bar },
  setup() {
    const job_store = useJobStore()

    const survival_data = {
      labels: ["6months", "12months","18months", "24months"],
      datasets: [
        {
          label: "Recurrence free survival rate",
          data: job_store.survival_rates,
          backgroundColor: '#1976d2',
        },
      ]
    }

    const chart_options = {
      responsive: true,
      scales: {
        y: {
          beginAtZero: true,
          suggestedMax: 1
        }
      }
    };
    function getStatusClass(value) {
      // 根据 value 决定返回哪个 CSS 类
      if (value > 90) return 'high';
      if (value > 50) return 'medium';
      return 'low';
    }
    return {job_store, survival_data, chart_options,getStatusClass}
  },
}
</script>
<style>
.status-indicators {
  display: flex;
}
.status-circle {
  width: 15px;
  height: 15px;
  border-radius: 50%;
  margin-right: 4px;
}
.status-circle.red {
  background-color: #FF0000;
}

.status-circle.yellow {
  background-color: #FFFF00;
}

.status-circle.green {
  background-color: #00FF00;
}
.custom-bg {
  background-color: rgba(255, 255, 255, 0.7); /* 白色背景，50% 透明度 */ 
  
}
</style>