<script>
	export let todo, updateTodo, deleteTodo, today;
	import { blur, crossfade, draw, fade, fly, scale, slide } from 'svelte/transition';

	let isDeleted = false;
</script>

<div class={isDeleted ? 'todos deletor' : 'todos'} class:done={todo.isComplete}>
	<!-- transition:slide={{ duration: 350 }} -->

	<!-- transition:fly={{ x: 30 }} -->

	<!-- if todo is complete adds call done -->
	<input
		type="checkbox"
		checked={todo.isComplete}
		disabled={todo.isComplete}
		on:change={(e) => {
			todo.isComplete = e.currentTarget.checked;
			updateTodo(todo);
		}}
	/>
	<!-- <input
		type="text"
		value={todo.task}
		on:input={(e) => {
			todo.task = e.currentTarget.value;
			updateTodo(todo);
		}}
	/> -->
	<span>{todo.task} </span>
	<p>{today == todo.date ? 'today' : 'overdue'}</p>
	<!-- <button on:click={() => deleteTodo(todo)}> Delete </button> -->
	<button
		class="btn-delete"
		on:click={() => {
			deleteTodo(todo);
		}}
	>
		Delete
	</button>
</div>

<!-- on:click={() => {
	(isDeleted = true),
		setTimeout(() => {
			deleteTodo(todo);
		}, 1000);
}} -->
<style>
	.todos {
		display: flex;
		margin-bottom: 10px;
		transition: all 1s ease-in-out;
	}
	.done {
		opacity: 0.5;
	}
	/* .done input[type='text'] {
		text-decoration: line-through;
	} */

	.btn-delete {
		background: red;
	}

	.deletor {
		/* animation: fade-in-keyframes 1s; */
		opacity: 0;
	}
	@keyframes fade-in-keyframes {
		from {
			opacity: 1;
		}
		to {
			opacity: 0;
		}
	}

	/* .successfully-saved.hide-opacity {
		opacity: 0;
	}

	.successfully-saved {
		color: #ffffff;
		text-align: center;

		-webkit-transition: opacity 1s ease-in-out;
		-moz-transition: opacity 1s ease-in-out;
		-ms-transition: opacity 1s ease-in-out;
		-o-transition: opacity 1s ease-in-out;
		opacity: 1;
	} */
</style>
