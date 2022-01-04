<script>
	export let todo, updateTodo, deleteTodo;
</script>

<div class="todos" class:done={todo.isComplete}>
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
