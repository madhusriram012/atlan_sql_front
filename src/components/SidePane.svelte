<script lang="ts">
    import {createEventDispatcher} from 'svelte';
    import { onMount } from 'svelte';
	import Environments from './Environments.svelte';
	import NewQuery from './NewQuery.svelte';
    export let table: any;
    export let activeView: 'tables' | string;
    export let pickRandomTable:any;
    const dispatch = createEventDispatcher();

    function changeActiveView(param: string){
        dispatch('changeActiveView', param);
    }

    function changeActiveTableItem(param: number){
        dispatch('changeActiveTableItem', param);
    }

    function filterTable(param: number){
        dispatch('filterTable', param);
    }
    
    onMount(() => {      
        changeActiveView('tables');
    });
</script>

<aside class="Aside Col--j-start gap-10">

    <Environments/>

    <div class="Row--between gap-10 w-100" style="margin-top: 40px;">
        <!-- svelte-ignore a11y-click-events-have-key-events -->
        <!-- svelte-ignore a11y-no-noninteractive-element-interactions -->
        <h3
            class="w-100"
            style="text-align: center; cursor:pointer"
            on:click={() => changeActiveView('tables')}
        >
            Tables
        </h3>
        <hr style="height: 100%; margin: 0;" />
        <!-- svelte-ignore a11y-click-events-have-key-events -->
        <!-- svelte-ignore a11y-no-noninteractive-element-interactions -->
        <h3
            class="w-100"
            style="text-align: center; cursor:pointer"
            on:click={() => changeActiveView('history')}
        >
            History
        </h3>
        <hr style="height: 100%; margin: 0;" />
        <!-- svelte-ignore a11y-click-events-have-key-events -->
        <!-- svelte-ignore a11y-no-noninteractive-element-interactions -->
        <h3
            class="w-100"
            style="text-align: center; cursor:pointer"
            on:click={() => changeActiveView('saved')}
        >
            Saved
        </h3>
    </div>
    {#if activeView === 'tables'}
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
                        on:click={() => changeActiveTableItem(index)}
                        href={`#${item.id}`}
                    >
                        {item.name}
                    </a>
                    <button
                        class="FancyButton"
                        data-icon={String.fromCharCode(58829)}
                        on:click={() => filterTable(item.id)}
                    />
                </li>
            {/each}
        </ul>
    {:else if activeView === 'history'}
        <h2>History</h2>
        <em>No history yet</em>
    {:else}
        <h2>Saved</h2>
        <em>No saved queries yet</em>
    {/if}
<NewQuery on:pickRandomTable={pickRandomTable}/>
</aside>