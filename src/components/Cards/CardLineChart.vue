<template>
	<a-card :bordered="false" class="dashboard-bar-line header-solid">
	  <template #title>
		<h6>Platillos Más Consumidos</h6>
		<p>Consumo por platillo en el mes</p>
	  </template>
	  <chart-line :height="310" :data="lineChartData"></chart-line>
	</a-card>
  </template>
  
  <script>
  import axios from 'axios';
  import ChartLine from '../Charts/ChartLine';
  
  export default ({
	components: {
	  ChartLine,
	},
	data() {
	  return {
		lineChartData: {
		  labels: [], // Se llenará con los nombres de los platillos
		  datasets: [{
			label: "Total Consumido",
			tension: 0.4,
			borderWidth: 3,
			pointRadius: 0,
			borderColor: "#1890FF",
			data: [], // Se llenará con el total_consumido
			maxBarThickness: 6
		  }],
		},
	  }
	},
	mounted() {
	  this.fetchData();
	},
	methods: {
	  async fetchData() {
		try {
		  const response = await axios.get(
			"https://app-y36l.onrender.com/platillos-mas-consumidos"
		  );
		  const data = response.data;
  
		  // Procesar los datos para el gráfico
		  const labels = data.map(item => item.nombreplatillo);
		  const values = data.map(item => item.total_consumido);
  
		  // Actualizar el gráfico
		  this.lineChartData.labels = labels;
		  this.lineChartData.datasets[0].data = values;
  
		} catch (error) {
		  console.error("Error al obtener los datos de la API:", error);
		}
	  },
	},
  })
  </script>
  
  <style scoped>
  .dashboard-bar-line {
	min-height: 500px;
  }
  
  .header-solid h6 {
	margin-bottom: 10px;
  }
  </style>