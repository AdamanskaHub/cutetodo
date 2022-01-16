<script>
	export let taskCount;
</script>

<p>nicehcu</p>
{#if taskCount === 0}
	<p>task 0</p>
{:else if taskCount === 1}
	<p>task 1</p>
{:else}
	<p>no condition</p>
{/if}
