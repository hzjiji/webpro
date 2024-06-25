<template>
  <!-- 图表容器 -->
  <div class="chart-container" ref="chartContainer"></div>
</template>

<script>
import axios from "axios";
import echarts from 'echarts';
import router from "@/router/router-static"; // 引入路由实例
import { Mounted,ref} from 'vue';

export default {
  name: "OrderChart",
  mounted() {
    // 在 mounted 钩子中进行初始化
    this.checkTokenAndFetchData();
  },
  methods: {
    checkTokenAndFetchData() {
      if (!this.$storage.get("Token")) {
        router.push({ name: "login" });
        return;
      }

      // 如果有 Token，继续获取数据
      this.fetchChartData();
    },
    fetchChartData() {
      axios
          .get("http://localhost:8080/springbootniyfl/orders/completed-orders-by-type")
          .then((response) => {
            if (response.data.success) {
              this.chartData = response.data.data;
              this.initChart();
            } else {
              console.error("Failed to fetch data:", response.data.message);
            }
          })
          .catch((error) => {
            console.error("Request failed:", error);
          });
    },
    initChart() {
      const chart = echarts.init(this.$refs.chartContainer);

      const option = {
        title: {
          text: "已完成订单按商品类型统计",
          left: "center",
        },
        tooltip: {},
        xAxis: {
          data: this.chartData.map((item) => item["商品类型"]),
        },
        yAxis: {},
        series: [
          {
            name: "数量",
            type: "bar",
            data: this.chartData.map((item) => item["数量"]),
          },
        ],
      };

      chart.setOption(option);
      window.addEventListener("resize", () => {
        chart.resize();
      });
    },
  },
  data() {
    return {
      chartData: [], // 存储图表数据
    };
  },
};
</script>

<style scoped>
.chart-container {
  width: 600px;
  height: 400px;
  margin: 0 auto;
}
</style>