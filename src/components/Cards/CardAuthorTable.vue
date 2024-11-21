<template>
	<a-card :bordered="false" class="header-solid h-full" :bodyStyle="{padding: 0,}">
	  <template #title>
		<a-row type="flex" align="middle">
		  <a-col :span="24" :md="12">
			<h6>{{ title }}</h6>
			<p>{{ subtitle }}</p>
		  </a-col>
		</a-row>
	  </template>
	  
	  <a-table
		:columns="columns"
		:data-source="data"
		:pagination="false"
	  >
		<!-- Puedes personalizar cómo se muestran los datos usando slots si lo necesitas -->
		<template slot="nummesa" slot-scope="text">
		  <span class="font-semibold">Mesa #{{ text }}</span>
		</template>
  
		<template slot="total_usos" slot-scope="text">
		  <span class="font-semibold">{{ text }} usos</span>
		</template>
  
		<template slot="total_clientes" slot-scope="text">
		  <span class="font-semibold">{{ text }} clientes</span>
		</template>
	  </a-table>
	</a-card>
  </template>
  
  <script>
  export default {
	props: {
	  data: {
		type: Array,
		default: () => [],
	  },
	  columns: {
		type: Array,
		default: () => [],
	  },
	  title: {
		type: String,
		default: '',
	  },
	  subtitle: {
		type: String,
		default: '',
	  },
	},
	data() {
	  return {
		// Si necesitas algún estado local
	  }
	},
  }
  </script>
  
  <style scoped>
  .header-solid {
	background: #fff;
	border-radius: 8px;
  }
  
  .header-solid h6 {
	margin: 0;
	font-size: 16px;
	font-weight: 600;
  }
  
  .header-solid p {
	margin: 0;
	color: #8c8c8c;
  }
  
  /* Agrega más estilos según necesites */
  </style>