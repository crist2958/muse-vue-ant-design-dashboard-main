<template>
    <a-card :bordered="false" class="dashboard-bar-chart">
      <chart-bar :height="220" :data="barChartData"></chart-bar>
    
    </a-card>
  </template>
  
  <script>
  import axios from 'axios';
  import ChartBar from '../Charts/ChartBar';
  
  export default {
    components: {
      ChartBar,
    },
    data() {
      return {
        clientesList: [], // Nueva propiedad para almacenar la lista de clientes
        barChartData: {
          labels: [], 
          datasets: [
            {
              label: "Pedidos",
              backgroundColor: "#fafafa",
              borderWidth: 0,
              borderRadius: 6,
              data: [], 
              maxBarThickness: 20,
            },
          ],
        },
      };
    },
    mounted() {
      this.fetchData();
    },
    methods: {
      async fetchData() {
        try {
          const response = await axios.get(
            "https://app-y36l.onrender.com//clientes-frecuentes"
          );
          const data = response.data;
  
          // Guardar la lista de clientes
          this.clientesList = data;
  
          // Procesar los datos para el gráfico
          const labels = data.map((item) => `${item.nombre}`);
          const values = data.map((item) => item.total_pedidos);
  
          // Actualizar el gráfico
          this.barChartData.labels = labels;
          this.barChartData.datasets[0].data = values;
        } catch (error) {
          console.error("Error al obtener los datos de la API:", error);
        }
      },
    },
  };
  </script>
  
  <style scoped>
  .clientes-list {
    margin-top: 10px;
  }
  
  .clientes-list p {
    margin: 5px 0;
    font-size: 14px;
  }
  </style>