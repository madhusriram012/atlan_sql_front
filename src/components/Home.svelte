<script lang="ts">
	import { createEventDispatcher } from 'svelte';
	import DataResult from './DataResult.svelte';
	import ScehmaResult from './ScehmaResult.svelte';
	import Editor from './Editor.svelte';
	import QueryOptionsView from './QueryOptionsView.svelte';
	export let currentItem: any;
	export let currentItemData: any;
	export let activeTab: any;

	const dispatch = createEventDispatcher();
	function runQuery() {
		dispatch('runQuery');
	}

	function changeActiveTab(param: string) {
		dispatch('changeActiveTab', param);
	}

	function pickRandomTable() {
		dispatch('pickRandomTable');
	}
</script>

<main class="Main Col--j-start gap-15">
	{#if currentItem}
		<div class="Col--a-end gap-15 w-100">
			<Editor {currentItem} />
			<div class="Col--j-end gap-15 w-100">
				<div class="Row--between gap-15 w-100">
					<span class="Row--start">
						About {currentItemData ? currentItemData.noOfRows : 0} results ({currentItemData
							? currentItemData.time
							: 0} seconds)
					</span>
					<QueryOptionsView on:runQuery={runQuery} />
				</div>
			</div>
		</div>
		<div class="Tabs Row--between">
			<button class:active={activeTab === 'data'} on:click={() => changeActiveTab('data')}>
				Data
			</button>
			<button class:active={activeTab === 'schema'} on:click={() => changeActiveTab('schema')}>
				Schema
			</button>
		</div>
		{#if activeTab === 'data'}
			<div class="DataGrid">
				{#if currentItemData === null}
					<div class="Row--center w-100">
						<h3>Run the query to see the results</h3>
					</div>
				{:else}
					<ScehmaResult {currentItemData} />
				{/if}
			</div>
		{:else}
			<div class="DataGrid">
				{#if currentItemData === null}
					<div class="Row--center w-100">
						<h3>Run the query to see the results</h3>
					</div>
				{:else}
					<DataResult {currentItemData} />
				{/if}
			</div>
		{/if}
	{:else}
		<div class="Col--center w-100 gap-15" style="height: 100%; ">
			<h2
				class="Col--center gap-15"
				style="padding: 40px; height: 150px; border-radius: 5px; background-color: #202123; color: #fff;"
			>
				Select a query from the table or create a new one
				<button class="FancyButton" on:click={pickRandomTable}> New Query </button>
			</h2>
		</div>
	{/if}
</main>
