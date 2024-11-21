<template>
	<a-card :bordered="false" class="header-solid h-full" :bodyStyle="{padding: '20px'}">
		<template #title>
			<h6>Mesa Más Usada</h6>
		</template>
		<div>
			<p><strong>Número de Mesa:</strong> {{ mesaData.nummesa }}</p>
			<p><strong>Total de Usos:</strong> {{ mesaData.total_usos }}</p>
		</div>
	</a-card>
</template>

<script>
export default {
	props: {
		mesaData: {
			type: Object,
			default: () => ({
				nummesa: 'Desconocido',
				total_usos: 'Desconocido',
			}),
		},
	},
};
</script>
