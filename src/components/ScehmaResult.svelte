<script lang="ts">
	import Grid from 'gridjs-svelte';
	export let currentItemData: any;
</script>

<Grid
	data={currentItemData.data}
	sort
	search
	pagination={{ enabled: true, limit: 5 }}
	className={{
		paginationButton: 'paginationButton'
	}}
	style={{
		td: {
			backgroundColor: '#202123',
			border: '1px solid #4d5055'
		},

		th: {
			backgroundColor: '#202123',
			border: '1px solid #4d5055'
		},
		footer: {
			backgroundColor: '#202123',
			'border-top': '1px solid #4d5055'
		},
		paginationButton: {
			backgroundColor: '#202123',
			'border-top': '1px solid #4d5055'
		}
	}}
/>
