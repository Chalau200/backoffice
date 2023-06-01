<template>
  <div>
    
    <div class="flex bg-gray-100 rounded shadow p-4">
    <div class="mr-8">
      <p class="text-lg font-semibold">Utilizadores:</p>
      <p class="text-xl">{{ utilizadorCount }}</p>
    </div>
    <div class="mr-8">
      <p class="text-lg font-semibold">Funcionários:</p>
      <p class="text-xl">{{ funcionariosCount }}</p>
    </div>
    <div class="mr-8">
      <p class="text-lg font-semibold">Oportunidades:</p>
      <p class="text-xl">{{ itensDetalheCount }}</p>
    </div>
    <div class="mr-8">
      <p class="text-lg font-semibold">Marcações:</p>
      <p class="text-xl">{{ marcacaoCount }}</p>
    </div>
    <div>
      <p class="text-lg font-semibold">Doações:</p>
      <p class="text-xl">{{ TodasDoacoesCount }}</p>
    </div>
  </div>
    <!-- Charts -->
    <div class="row">
      

      <div class="col-md-6 col-12">
        <chart-card
          title="Estado das Marcações"
         
          :chart-data="filteredData"
          chart-type="Pie"
        >
          
          
        </chart-card>
      </div>

     
    </div>
  </div>
</template>

<script>





import { StatsCard, ChartCard } from "@/components/index";
import Chartist from "chartist";
import ChartistLegend from "chartist-plugin-legend";

export default {
  components: {
    StatsCard,
    ChartCard,
  },
  data() {
    return {
      utilizadorCount: 0,
      itensDetalheCount: 0,
      marcacaoCount: 0,
      TodasDoacoesCount: 0,

      statsCards: [
        {
          type: "warning",
          icon: "ti-server",
          title: "Capacity",
          value: "105GB",
          footerText: "Updated now",
          footerIcon: "ti-reload",
        },
        {
          type: "success",
          icon: "ti-wallet",
          title: "Revenue",
          value: "$1,345",
          footerText: "Last day",
          footerIcon: "ti-calendar",
        },
        {
          type: "danger",
          icon: "ti-pulse",
          title: "Errors",
          value: "23",
          footerText: "In the last hour",
          footerIcon: "ti-timer",
        },
        {
          type: "info",
          icon: "ti-twitter-alt",
          title: "Followers",
          value: "+45",
          footerText: "Updated now",
          footerIcon: "ti-reload",
        },
      ],
      usersChart: {
        data: {
          labels: [
            "9:00AM",
            "12:00AM",
            "3:00PM",
            "6:00PM",
            "9:00PM",
            "12:00PM",
            "3:00AM",
            "6:00AM",
          ],
          series: [
            [287, 385, 490, 562, 594, 626, 698, 895, 952],
            [67, 152, 193, 240, 387, 435, 535, 642, 744],
            [23, 113, 67, 108, 190, 239, 307, 410, 410],
          ],
        },
        options: {
          low: 0,
          high: 1000,
          showArea: true,
          height: "245px",
          axisX: {
            showGrid: false,
          },
          lineSmooth: Chartist.Interpolation.simple({
            divisor: 3,
          }),
          showLine: true,
          showPoint: false,
        },
      },
      activityChart: {
        data: {
          labels: [
            "Jan",
            "Feb",
            "Mar",
            "Apr",
            "Mai",
            "Jun",
            "Jul",
            "Aug",
            "Sep",
            "Oct",
            "Nov",
            "Dec",
          ],
          series: [
            [542, 543, 520, 680, 653, 753, 326, 434, 568, 610, 756, 895],
            [230, 293, 380, 480, 503, 553, 600, 664, 698, 710, 736, 795],
          ],
        },
        options: {
          seriesBarDistance: 10,
          axisX: {
            showGrid: false,
          },
          height: "245px",
        },
      },
      preferencesChart: {
        data: {
          labels: ["62%", "32%", "6%"],
          series: [62, 32, 6],
        },
        options: {},
      },
    };
  },
  mounted() {
    this.calculateCounts();
  },
  methods: {
    calculateCounts() {
      const utilizadorData = localStorage.getItem('utilizador');
      if (utilizadorData) {
        const utilizador = JSON.parse(utilizadorData);
        this.utilizadorCount = utilizador.length;
      }

      const itensDetalheData = localStorage.getItem('ItensDetalhe');
      if (itensDetalheData) {
        const itensDetalhe = JSON.parse(itensDetalheData);
        this.itensDetalheCount = itensDetalhe.length;
      }

      const marcacaoData = localStorage.getItem('marcacao');
      if (marcacaoData) {
        const marcacao = JSON.parse(marcacaoData);
        this.marcacaoCount = marcacao.length;
      }

      const funcionariosData = localStorage.getItem('funcionarios');
      if (funcionariosData) {
        const funcionarios= JSON.parse(funcionariosData);
        this.funcionariosCount = funcionarios.length;
      }

      const TodasDoacoesData = localStorage.getItem('TodasDoacoes');
      if (TodasDoacoesData) {
        const TodasDoacoes= JSON.parse(TodasDoacoesData);
        this.TodasDoacoesCount = TodasDoacoes.length;
      }

    },
  },
  computed: {
    filteredData() {
      const data = JSON.parse(localStorage.getItem("marcacao"));
      const filteredData = data.filter((item) =>
        ["Aceite", "Rejeitado", "Pendente"].includes(item.estado)
      );

      const totalCount = filteredData.length;

      const labels = [];
      const series = [];

      ["Aceite", "Rejeitado", "Pendente"].forEach((category) => {
        const count = filteredData.filter((item) => item.estado === category)
          .length;
        const percentage =
          totalCount > 0 ? ((count / totalCount) * 100).toFixed(2) : 0;

        if (count > 0) {
          labels.push(`${category} (${percentage}%)`);
          series.push(count);
        }
      });

      return {
        labels,
        series,
      };
    },
  },
};
</script>



<style scoped>
.flex {
  display: flex;
  justify-content: space-between;
  align-items: center;
  margin-bottom: 20px;
  background-color: #ffffff;
}
</style>
