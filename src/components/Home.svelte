<script lang="ts">
	import { createEventDispatcher } from 'svelte';
    import Grid from 'gridjs-svelte';
    export let currentItem:any;
    export let currentItemData:any;
    export let activeTab:any;

	const dispatch = createEventDispatcher();
	function runQuery() {
		dispatch('runQuery');
	}

    function changeActiveTab(param: string){
        dispatch('changeActiveTab', param);
    }

    function pickRandomTable(){
        dispatch('pickRandomTable');
    }
</script>

<main class="Main Col--j-start gap-15">
	{#if currentItem}
		<div class="Col--a-end gap-15 w-100">
			<textarea
				class="FancyTextarea"
				placeholder="Enter your dummy query here..."
				bind:value={currentItem['query']}
			/>
			<div class="Col--j-end gap-15 w-100">
				<div class="Row--between gap-15 w-100">
					<span class="Row--start">
						About {currentItemData ? currentItemData.noOfRows : 0} results ({currentItemData
							? currentItemData.time
							: 0} seconds)
					</span>
					<div class="Row--j-end gap-15" style="width: 600px;">
						<button class="FancyButton" data-icon={String.fromCharCode(57697)} on:click={() => {}}>
							Save Query
						</button>
						<button class="FancyButton" data-icon={String.fromCharCode(59574)} on:click={runQuery}>
							Run Query
						</button>
						<details class="FancyMenu">
							<summary style="height: 40px;">Download</summary>
							<ul class="FancyMenu__content" data-align="right" style="width: 150px;">
								<button class="FancyButton" style="width: 100%;" aria-label="Close"> JSON </button>
								<button class="FancyButton" style="width: 100%;" aria-label="Close"> CSV </button>
								<button class="FancyButton" style="width: 100%;" aria-label="Close"> XML </button>
							</ul>
						</details>
						<button
							class="FancyButton"
							style="width: auto;"
							data-icon={String.fromCharCode(59405)}
						/>
					</div>
				</div>
			</div>
		</div>
		<div class="Tabs Row--between">
			<button
				class:active={activeTab === 'data'}
				on:click={() => changeActiveTab('data')}
			>
				Data
			</button>
			<button
				class:active={activeTab === 'schema'}
				on:click={() => changeActiveTab('schema')}
			>
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
				{/if}
			</div>
		{:else}
			<div class="DataGrid">
				{#if currentItemData === null}
					<div class="Row--center w-100">
						<h3>Run the query to see the results</h3>
					</div>
				{:else}
					<Grid
						data={currentItemData.schema}
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
				<button
					class="FancyButton"
					on:click={pickRandomTable}
				>
					New Query
				</button>
			</h2>
		</div>
	{/if}
</main>
