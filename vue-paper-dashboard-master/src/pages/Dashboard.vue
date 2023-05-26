<template>
  <div class="dashboard">
    <h1>Dashboard</h1>
    <div class="stats-card" v-for="(estadoPercentage, estado) in estadoPercentages" :key="estado">
      <div class="title">{{ estado }} Percentage</div>
      <div class="value">{{ estadoPercentage }}%</div>
    </div>
  </div>
</template>

<script>
export default {
  data() {
    return {
      estadoPercentages: {},
    };
  },
  mounted() {
    const marcacaoItems = JSON.parse(localStorage.getItem("marcacao") || "[]");

    const estadoCounts = marcacaoItems.reduce((counts, item) => {
      const estado = item.estado;
      counts[estado] = (counts[estado] || 0) + 1;
      return counts;
    }, {});

    const totalMarcacaoItems = marcacaoItems.length;

    for (const estado in estadoCounts) {
      const estadoCount = estadoCounts[estado];
      const estadoPercentage = (estadoCount / totalMarcacaoItems) * 100;
      this.$set(this.estadoPercentages, estado, estadoPercentage.toFixed(2));
    }
  },
};
</script>

<style>
.dashboard {
  display: flex;
  flex-direction: column;
  align-items: center;
  justify-content: center;
  height: 100vh;
}

.stats-card {
  background-color: #f5f5f5;
  border-radius: 8px;
  padding: 20px;
  text-align: center;
  margin-bottom: 10px;
}

.title {
  font-size: 18px;
  font-weight: bold;
  margin-bottom: 10px;
}

.value {
  font-size: 24px;
  font-weight: bold;
}
</style>
