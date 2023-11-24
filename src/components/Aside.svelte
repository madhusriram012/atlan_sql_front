<script lang="ts">
    import {createEventDispatcher} from 'svelte';
    export let table: any;
    import { onMount } from 'svelte';
    export let activeView: 'tables' | string;

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

    function pickRandomTable(){
        dispatch('pickRandomTable');
    }
    onMount(() => {
        
        changeActiveView('tables');
    });
</script>

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
    <div style="margin-top: auto; background-color: black;" class="Col--center w-100 gap-10">
        <button
            class="FancyButton w-100"
            on:click={pickRandomTable}
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