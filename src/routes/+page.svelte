<script lang="ts">
	import { onMount } from 'svelte';
	// @ts-ignore
	import { parse } from 'papaparse';

	// @ts-ignore
	import toJsonSchema from 'to-json-schema';

	import Aside from '../components/Aside.svelte';
	import Home from '../components/Home.svelte';
	$: currentItem = null as (typeof table)[0] | null;
	$: currentItemData = null as any;
	$: activeTab = 'data';
	$: activeView = 'table';

	function changeActiveView(e: Event & { detail: string }) {
		activeView = e.detail;
	}

	const changeActiveTableItem = (e: Event & { detail: number }) => {
		let index = e.detail;
		table.forEach((item) => {
			item.isActive = false;
		});
		table[index].isActive = true;
		currentItem = table[index];
		currentItemData = null;
	};

	const filterTable = (e: Event & { detail: number }) => {
		let itemID = e.detail;
		table = table.filter((item: any) => item.id !== itemID);
		if (table.length) {
			window.location.hash = `#${table[table.length - 1].id}`;
		} else {
			window.location.hash = '';
		}
	};

	const pickRandomTable = () => {
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
	};

	const runQuery = () => {
		if (currentItem) {
			// fetch the data from the source and pass it to the main component
			fetchData(currentItem).then((data) => {
				currentItemData = data;
				console.log(data.schema);
			});
		}
	};

	const changeActiveTab = (e: Event & { detail: string }) => {
		activeTab = e.detail;
	};

	import { tableOrig } from '../data/tables';

	$: table = tableOrig;
	import { sourceList } from '../data/tables';

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
</script>

<div class="Home">
	<Aside
		{table}
		{activeView}
		on:changeActiveView={changeActiveView}
		on:changeActiveTableItem={changeActiveTableItem}
		on:filterTable={filterTable}
		on:pickRandomTable={pickRandomTable}
	/>
	<Home
		{currentItem}
		{activeTab}
		{currentItemData}
		on:runQuery={runQuery}
		on:changeActiveTab={changeActiveTab}
		on:pickRandomTable={pickRandomTable}
	/>
</div>

<style lang="scss" global>
	@import '../data/styles.scss';
</style>
