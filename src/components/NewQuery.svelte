<script lang="ts">
    import {createEventDispatcher} from 'svelte';
    const dispatch = createEventDispatcher();
	function pickRandomTable() {
		dispatch('pickRandomTable');
	}
</script>

<div style="margin-top: auto; background-color: black;" class="Col--center w-100 gap-10">
	<button class="FancyButton w-100" on:click={pickRandomTable}> New Query </button>
	<div class="Row--between gap-15 w-100">
		<div class="Row--start Profile gap-5">
			<img src="./avatar_logo.jpeg" alt="User" class="UserImage" width="40px" height="40px" />
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
