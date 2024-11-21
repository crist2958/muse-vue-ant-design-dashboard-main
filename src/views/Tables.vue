<!-- Tables.vue -->
<template>
	<div>
	  <!-- Mesa más usada Table -->
	  <a-row :gutter="24" type="flex">
		<a-col :span="24" class="mb-24">
		  <CardAuthorTable
			:data="mesasData"
			:columns="mesasColumns"
			title="Mesas Más Utilizadas"
			subtitle="Registro de uso de mesas"
		  ></CardAuthorTable>
		</a-col>
	  </a-row>
  
	  <!-- Mes con más clientes Table -->
	  <a-row :gutter="24" type="flex">
		<a-col :span="24" class="mb-24">
		  <CardAuthorTable
			:data="clientesData"
			:columns="clientesColumns"
			title="Mes con Más Clientes"
			subtitle="Registro mensual de clientes"
		  ></CardAuthorTable>
		</a-col>
	  </a-row>
	</div>
  </template>
  
  <script>
  import axios from 'axios';
  import CardAuthorTable from '../components/Cards/CardAuthorTable';
  
  // Definición de columnas para la tabla de mesas
  const mesasColumns = [
	{
	  title: 'NÚMERO DE MESA',
	  dataIndex: 'nummesa',
	  class: 'text-muted',
	},
	{
	  title: 'TOTAL DE USOS',
	  dataIndex: 'total_usos',
	  class: 'text-muted',
	},
  ];
  
  // Definición de columnas para la tabla de clientes por mes
  const clientesColumns = [
	{
	  title: 'MES',
	  dataIndex: 'mes',
	  class: 'text-muted',
	},
	{
	  title: 'TOTAL DE CLIENTES',
	  dataIndex: 'total_clientes',
	  class: 'text-muted',
	},
  ];
  
  export default {
	components: {
	  CardAuthorTable,
	},
	data() {
	  return {
		mesasData: [],
		clientesData: [],
		mesasColumns: mesasColumns,
		clientesColumns: clientesColumns,
	  }
	},
	mounted() {
	  this.fetchMesasData();
	  this.fetchClientesData();
	},
	methods: {
	  async fetchMesasData() {
		try {
		  const response = await axios.get('https://app-y36l.onrender.com/mesa-mas-usada');
		  this.mesasData = response.data.map(item => ({
			...item,
			key: item.nummesa.toString()
		  }));
		} catch (error) {
		  console.error('Error al obtener datos de mesas:', error);
		}
	  },
	  formatearMes(fecha) {
		const meses = [
		  'enero', 'febrero', 'marzo', 'abril', 'mayo', 'junio',
		  'julio', 'agosto', 'septiembre', 'octubre', 'noviembre', 'diciembre'
		];
		
		try {
		  const date = new Date(fecha);
		  if (isNaN(date.getTime())) {
			throw new Error('Fecha inválida');
		  }
		  
		  const mes = meses[date.getMonth()];
		  const año = date.getFullYear();
		  return `${mes} ${año}`;
		} catch (error) {
		  console.error('Error al formatear fecha:', error);
		  return 'Fecha no disponible';
		}
	  },
	  async fetchClientesData() {
		try {
		  const response = await axios.get('https://app-y36l.onrender.com/mes-mas-clientes');
		  console.log('Respuesta de la API:', response.data);
		  
		  if (response.data && response.data.length > 0) {
			const primerRegistro = response.data[0];
			const mesFormateado = this.formatearMes(primerRegistro.mes);
			
			this.clientesData = [{
			  key: '1',
			  mes: mesFormateado,
			  total_clientes: primerRegistro.total_clientes
			}];
		  } else {
			this.clientesData = [{
			  key: '1',
			  mes: 'No hay datos disponibles',
			  total_clientes: 0
			}];
		  }
		} catch (error) {
		  console.error('Error al obtener datos de clientes por mes:', error);
		  this.clientesData = [{
			key: '1',
			mes: 'Error al cargar datos',
			total_clientes: 0
		  }];
		}
	  }
	}
  }
  </script>