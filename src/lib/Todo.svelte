<script>
	export let todo, updateTodo, deleteTodo, today;
	import { blur, crossfade, draw, fade, fly, scale, slide } from 'svelte/transition';
</script>

<div class="todos" class:done={todo.isComplete} transition:slide={{ duration: 350 }}>
	<!-- transition:fly={{ x: 30 }} -->

	<!-- if todo is complete adds call done -->
	<input
		type="checkbox"
		checked={todo.isComplete}
		on:change={(e) => {
			todo.isComplete = e.currentTarget.checked;
			updateTodo(todo);
		}}
	/>
	<input
		type="text"
		value={todo.task}
		on:input={(e) => {
			todo.task = e.currentTarget.value;
			updateTodo(todo);
		}}
	/>
	<p>{today == todo.date ? 'today' : 'overdue'}</p>
	<button on:click={() => deleteTodo(todo)}> Delete </button>
</div>

<style>
	.todos {
		display: flex;
		margin-bottom: 10px;
	}
	.done {
		opacity: 0.5;
	}
	.done input[type='text'] {
		text-decoration: line-through;
	}
</style>
