<script lang="ts">
    import { createEventDispatcher } from 'svelte';

    const dispatch = createEventDispatcher();
    
	function runQuery() {
		dispatch('runQuery');
	}
</script>

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