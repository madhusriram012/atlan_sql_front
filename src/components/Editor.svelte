<script lang="ts">
	export let currentItem: any;
</script>

<textarea
	class="FancyTextarea"
	placeholder="Enter your dummy query here..."
	bind:value={currentItem['query']}
/>
