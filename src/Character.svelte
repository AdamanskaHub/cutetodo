<script>
	export let taskCount, todos;
</script>

{#if todos[0]}
	<p>date of 1st todo {todos[0].date}</p>
{/if}

{#if taskCount === 0}
	<p>task 0</p>
{:else if taskCount === 1}
	<p>task 1</p>
{:else}
	<p>no condition</p>
{/if}
