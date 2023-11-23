<script lang="ts">
	import { onMount } from 'svelte';
	// @ts-ignore
	import { parse } from 'papaparse';
	import Grid from 'gridjs-svelte';
	// @ts-ignore
	import toJsonSchema from 'to-json-schema';
	const version = 'v1';
	const sourceList = [
		{
			query: 'SELECT * FROM employee_territories',
			link: 'https://raw.githubusercontent.com/graphql-compose/graphql-compose-examples/master/examples/northwind/data/csv/employee_territories.csv'
		},
		{
			query: 'SELECT * FROM order_details',
			link: 'https://raw.githubusercontent.com/graphql-compose/graphql-compose-examples/master/examples/northwind/data/csv/order_details.csv'
		},
		{
			query: 'SELECT * FROM orders',
			link: 'https://raw.githubusercontent.com/graphql-compose/graphql-compose-examples/master/examples/northwind/data/csv/orders.csv'
		},
		{
			query: 'SELECT * FROM products',
			link: 'https://raw.githubusercontent.com/graphql-compose/graphql-compose-examples/master/examples/northwind/data/csv/products.csv'
		},
		{
			query: 'SELECT * FROM regions',
			link: 'https://raw.githubusercontent.com/graphql-compose/graphql-compose-examples/master/examples/northwind/data/csv/regions.csv'
		},
		{
			query: 'SELECT * FROM shippers',
			link: 'https://raw.githubusercontent.com/graphql-compose/graphql-compose-examples/master/examples/northwind/data/csv/shippers.csv'
		},
		{
			query: 'SELECT * FROM suppliers',
			link: 'https://raw.githubusercontent.com/graphql-compose/graphql-compose-examples/master/examples/northwind/data/csv/suppliers.csv'
		},
		{
			query: 'SELECT * FROM territories',
			link: 'https://raw.githubusercontent.com/graphql-compose/graphql-compose-examples/master/examples/northwind/data/csv/territories.csv'
		},
		{
			query: 'SELECT * FROM random_table',
			link: 'https://raw.githubusercontent.com/graphql-compose/graphql-compose-examples/master/examples/northwind/data/csv/random_table.csv'
		}
	];

	$: table = [
		{
			id: 1,
			name: 'Categories',
			isActive: true,
			query: 'SELECT * FROM categories',
			src: 'https://raw.githubusercontent.com/graphql-compose/graphql-compose-examples/master/examples/northwind/data/csv/categories.csv'
		},
		{
			id: 2,
			name: 'Customers',
			isActive: false,
			query: 'SELECT * FROM customers',
			src: 'https://raw.githubusercontent.com/graphql-compose/graphql-compose-examples/master/examples/northwind/data/csv/customers.csv'
		},
		{
			id: 3,
			name: 'Employees',
			isActive: false,
			query: 'SELECT * FROM employees',
			src: 'https://raw.githubusercontent.com/graphql-compose/graphql-compose-examples/master/examples/northwind/data/csv/employees.csv'
		},
		{
			id: 4,
			name: 'Orders',
			isActive: false,
			query: 'SELECT * FROM orders',
			src: 'https://raw.githubusercontent.com/graphql-compose/graphql-compose-examples/master/examples/northwind/data/csv/order_details.csv'
		},
		{
			id: 5,
			name: 'Territories',
			isActive: false,
			query: 'SELECT * FROM territories',
			src: 'https://raw.githubusercontent.com/graphql-compose/graphql-compose-examples/master/examples/northwind/data/csv/territories.csv'
		},
		{
			id: 6,
			name: 'Suppliers',
			isActive: false,
			query: 'SELECT * FROM suppliers',
			src: 'https://raw.githubusercontent.com/graphql-compose/graphql-compose-examples/master/examples/northwind/data/csv/suppliers.csv'
		},
		{
			id: 7,
			name: 'Shippers',
			isActive: false,
			query: 'SELECT * FROM shippers',
			src: 'https://raw.githubusercontent.com/graphql-compose/graphql-compose-examples/master/examples/northwind/data/csv/shippers.csv'
		}
	];

	// Get the item ID from the site hash query
	$: itemId = null as number | null;

	onMount(() => {
		const hash = window.location.hash;
		itemId = hash ? parseInt(hash.substring(1)) : null;
		table.forEach((item) => {
			item.isActive = false;
		});
	});

	// function that takes an item from the history array and fetches the data from the source
	const fetchData = async (item: (typeof table)[0]) => {
		const start = Date.now();
		const response = await fetch(item.src);
		const data = await response.text();
		return {
			data: parse(data, { header: true }).data,
			time: (Date.now() - start) / 1000,
			noOfRows: parse(data, { header: true }).data.length,
			schema: Object.entries(toJsonSchema(parse(data, { header: true }).data[0]).properties).map(
				([key, value]: any) => {
					return { key, value: value.type };
				}
			)
		};
	};

	$: currentItem = null as (typeof table)[0] | null;
	$: currentItemData = null as any;
	$: activeTab = 'data';
	$: activeView = 'table';
</script>

<div class="Home">
	<aside class="Aside Col--j-start gap-10">
		<h2>Environments</h2>
		
		<select class="FancySelect" aria-label="Select Environment">
			<option value="Development">Development</option>
			<option value="Staging">Staging</option>
			<option value="Production">Production</option>
		</select>

		<div class="Row--between gap-10 w-100" style="margin-top: 40px;">
			<!-- svelte-ignore a11y-click-events-have-key-events -->
			<!-- svelte-ignore a11y-no-noninteractive-element-interactions -->
			<h3
				class="w-100"
				style="text-align: center; cursor:pointer"
				on:click={() => {
					activeView = 'table';
				}}
			>
				Tables
			</h3>
			<hr style="height: 100%; margin: 0;" />
			<!-- svelte-ignore a11y-click-events-have-key-events -->
			<!-- svelte-ignore a11y-no-noninteractive-element-interactions -->
			<h3
				class="w-100"
				style="text-align: center; cursor:pointer"
				on:click={() => {
					activeView = 'History';
				}}
			>
				History
			</h3>
			<hr style="height: 100%; margin: 0;" />
			<!-- svelte-ignore a11y-click-events-have-key-events -->
			<!-- svelte-ignore a11y-no-noninteractive-element-interactions -->
			<h3
				class="w-100"
				style="text-align: center; cursor:pointer"
				on:click={() => {
					activeView = 'Saved';
				}}
			>
				Saved
			</h3>
		</div>
		{#if activeView === 'table'}
			<div class="Row--start gap-10 w-100">
				<input class="FancyInput" placeholder="Search tables..." />
				<button
					class="FancyButton"
					data-icon={String.fromCharCode(59574)}
					style="height: 100%; width: auto;"
				/>
			</div>
			<ul style="max-height: 100%; overflow-y: auto" class="Col--j-start gap-10 w-100">
				{#each table as item, index (item.id)}
					<li class="Aside--item Row--start" class:selected={item.isActive}>
						<a
							on:click={() => {
								table.forEach((item) => {
									item.isActive = false;
								});
								table[index].isActive = true;
								currentItem = table[index];
								currentItemData = null;
							}}
							href={`#${item.id}`}
						>
							{item.name}
						</a>
						<button
							class="FancyButton"
							data-icon={String.fromCharCode(58829)}
							on:click={() => {
								table = table.filter((_, i) => i !== index);
								if (table.length) {
									window.location.hash = `#${table[table.length - 1].id}`;
								} else {
									window.location.hash = '';
								}
							}}
						/>
					</li>
				{/each}
			</ul>
		{:else if activeView === 'History'}
			<h2>History</h2>
			<em>No history yet</em>
		{:else}
			<h2>Saved</h2>
			<em>No saved queries yet</em>
		{/if}
		<div style="margin-top: auto; background-color: black;" class="Col--center w-100 gap-10">
			<button
				class="FancyButton w-100"
				on:click={() => {
					const newId = table.length + 1;
					const randomQuery = sourceList[Math.floor(Math.random() * sourceList.length)];
					table.forEach((item) => {
						item.isActive = false;
					});
					table = [
						...table,
						{
							id: newId,
							name: `Query ${newId}`,
							isActive: true,
							query: '',
							src: randomQuery.link
						}
					];
					window.location.hash = `#${newId}`;

					currentItem = table[table.length - 1];
					currentItemData = null;
				}}
			>
				New Query
			</button>
			<div class="Row--between gap-15 w-100">
				<div class="Row--start Profile gap-5">
					<img src="./avatar_logo.jpeg" alt="User" class="UserImage" width="40px" height= "40px" />
					<div class="Col--a-start">
						<h3 class="Username">Jhon Doe</h3>
						<span class="UserEmail">
							<a href="mailto:jhon@doe">jhon@doe.com</a>
						</span>
					</div>
				</div>
				<span data-icon={String.fromCharCode(59834)} style="cursor: pointer;" />
			</div>
		</div>
	</aside>
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
						<div class="Row--j-end gap-15">
							<button
								class="FancyButton"
								data-icon={String.fromCharCode(59574)}
								on:click={() => {
									if (currentItem) {
										// fetch the data from the source and pass it to the main component
										fetchData(currentItem).then((data) => {
											currentItemData = data;
											console.log(data.schema);
										});
									}
								}}
							>
								Run Query
							</button>
							<details class="FancyMenu">
								<summary style="height: 40px;">Download</summary>
								<ul class="FancyMenu__content" data-align="right" style="width: 150px;">
									<button class="FancyButton" style="width: 100%;" aria-label="Close">
										JSON
									</button>
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
					on:click={() => {
						activeTab = 'data';
					}}
				>
					Data
				</button>
				<button
					class:active={activeTab === 'schema'}
					on:click={() => {
						activeTab = 'schema';
					}}
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
									// borderRadius: '5px'
								},

								th: {
									backgroundColor: '#202123',
									border: '1px solid #4d5055'
									// borderRadius: '5px 0 0 0'
								},
								footer: {
									backgroundColor: '#202123',
									'border-top': '1px solid #4d5055'
									// borderRadius: '5px'
								},
								paginationButton: {
									backgroundColor: '#202123',
									'border-top': '1px solid #4d5055'
									// borderRadius: '0 0 5px 5px'
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
									// borderRadius: '5px'
								},

								th: {
									backgroundColor: '#202123',
									border: '1px solid #4d5055'
									// borderRadius: '5px 0 0 0'
								},
								footer: {
									backgroundColor: '#202123',
									'border-top': '1px solid #4d5055'
									// borderRadius: '5px'
								},
								paginationButton: {
									backgroundColor: '#202123',
									'border-top': '1px solid #4d5055'
									// borderRadius: '0 0 5px 5px'
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
						on:click={() => {
							const newId = table.length + 1;
							const randomQuery = sourceList[Math.floor(Math.random() * sourceList.length)];
							table.forEach((item) => {
								item.isActive = false;
							});
							table = [
								...table,
								{
									id: newId,
									name: `Query ${newId}`,
									isActive: true,
									query: '',
									src: randomQuery.link
								}
							];
							window.location.hash = `#${newId}`;

							currentItem = table[table.length - 1];
							currentItemData = null;
						}}
					>
						New Query
					</button>
				</h2>
			</div>
		{/if}
	</main>
</div>

<style lang="scss" global>
	.Home {
		display: grid;
		@include box(100vw, 100vh);
		grid-template-columns: 340px 1fr;
		max-width: 100vw;
	}

	.Main {
		background-color: #343541;
		padding: 20px;
		overflow-x: auto;
		align-items: flex-start;

		span {
			color: #8c8c8c;
			font-size: 16px;
		}

		.FancyTextarea {
			@include box(100%, auto);
			min-height: 250px;
			border-radius: 5px;
			background-color: #202123;
			border: 1px solid #4d5055;
			color: #fff;
			font-family: 'Fira Code', monospace;
			font-size: 14px;
			padding: 20px;
			transition: all 0.2s ease-in-out;
			&::placeholder {
				color: #8c8c8c;
			}
			&:focus {
				border-color: #97b9fc;
			}
		}

		.Tabs {
			@include box(100%, auto);

			& > button {
				cursor: pointer;
				@include box(100%, 40px);
				background-color: #202123;
				color: #fff;
				border-top: 1px solid #4d5055;
				border-bottom: 1px solid #4d5055;

				transition: all 0.2s ease-in-out;

				&:first-child {
					border-radius: 5px 0 0 0;
					border-right: 1px solid #4d5055;
					border-left: 1px solid #4d5055;
				}

				&:last-child {
					border-radius: 0 5px 0 0;
					border-left: 0px solid #4d5055;
					border-right: 1px solid #4d5055;
				}

				&:hover {
					text-decoration: underline;
				}

				&.active {
					background-color: #4d5055;
					color: #fff;
					border-top: 1px solid #4d5055;
					border-bottom: 1px solid #4d5055;
					border-left: 1px solid #4d5055;
					border-right: 1px solid #4d5055;
				}
			}
		}
		& select {
			@include box(auto, 40px);
			background-color: #202123;
			border: 1px solid #4d5055;
		}
		& .FancyButton {
			@include box(300px, 40px);
			background-color: #97b9fc;
			border: 1px solid #4d5055;
			color: #000000;
			border-radius: 5px;
			transition: all 0.2s ease-in-out;

			position: sticky;
			bottom: 20px;
			&:hover {
				background-color: #4f72aa !important;
				color: #fff;
			}
		}

		.DataGrid {
			max-width: 100%;
			overflow-x: auto;

			input {
				color: #202123;
			}
		}
		.paginationButton {
			background-color: #202123 !important;
			color: #fff !important;
			border: 1px solid #4d5055 !important;
			&:hover {
				background-color: #4f72aa !important;
				color: #fff !important;
			}
		}
	}

	.Aside {
		background-color: #000;
		padding: 20px;
		position: relative;
		overflow-y: auto;
		& > select,
		& input {
			@include box(100%, 40px);
			background-color: #202123;

			border: 1px solid #4d5055;
		}

		& > select {
			flex-shrink: 0;
		}
		& input {
			border-radius: 5px;
			padding: 0 10px;
		}

		.Profile {
			& > img {
				width: 40px;
				height: 40px;
				border-radius: 50%;
				margin-right: 10px;
				aspect-ratio: 1/1;
			}
		}

		& > h2 {
			width: 100%;
			&:not(:first-child) {
				margin-top: 20px;
			}
		}

		&--item {
			@include box(100%, 40px);
			padding: 10px;
			color: #fff;
			border-radius: 5px;
			background-color: #202123;
			transition: all 0.2s ease-in-out;

			& > a {
				text-decoration: none;
				@include box(100%, 100%);
			}

			& > button {
				padding: 0;
				display: none;
				@include box(auto);
				background-color: transparent !important;
				transition: all 0.2s ease-in-out;
				&:hover {
					background-color: lightcoral !important;
					color: #fff;
				}
			}

			&:hover {
				background-color: #4d5055;
				& > button {
					display: block;
				}
			}

			&.selected {
				background-color: #4d5055;
			}

			&:last-child {
				margin-bottom: auto;
			}
		}

		& > div > .FancyButton {
			@include box(100%, 40px);
			margin-top: auto;
			background-color: #97b9fc;
			border: 1px solid #4d5055;
			color: #000000;
			border-radius: 5px;
			transition: all 0.2s ease-in-out;

			position: sticky;
			bottom: 20px;
			&:hover {
				background-color: #4f72aa !important;
				color: #fff;
			}
		}
	}
	@import 'https://cdn.jsdelivr.net/npm/gridjs/dist/theme/mermaid.min.css?version=${version}';
</style>
