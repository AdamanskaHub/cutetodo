<script>
	import supabase from '$lib/db';
	import { onMount } from 'svelte';
	import Todo from '$lib/Todo.svelte';

	let newTask = '';

	let todos = ['hfeiwh'];
	onMount(async () => {
		getallTodos();
	});

	const getallTodos = async () => {
		try {
			let { data, error } = await supabase.from('todos').select('*');
			todos = data;
		} catch (err) {
			console.log(err);
		}
	};

	const addTodo = async () => {
		try {
			const { data, error } = await supabase
				.from('todos')
				.insert([{ task: newTask, isComplete: false }]);
			await getallTodos();
			newTask = '';
		} catch (err) {
			console.log(err);
		}
	};

	const updateTodo = async (todo) => {
		try {
			const { data, error } = await supabase
				.from('todos')
				.update({ task: todo.task, isComplete: todo.isComplete })
				.eq('id', todo.id);
			await getallTodos();
		} catch (err) {
			console.log(err);
		}
		console.table(todo);
	};

	const deleteTodo = async (todo) => {
		try {
			const { data, error } = await supabase.from('todos').delete().eq('id', todo.id);
			await getallTodos();
		} catch (err) {
			console.log(err);
		}
		console.table(todo);
	};

	const handleEnter = (e) => {
		if (e.key === 'Enter' && newTask !== '') {
			addTodo();
		}
	};
</script>

<div class="add-todo">
	<input type="text" bind:value={newTask} />
	<button on:click={() => addTodo(newTask)}> Add task </button>
</div>

{#each todos as todo}
	<Todo {todo} {updateTodo} {deleteTodo} />
{:else}
	<p>no tasks</p>
{/each}

<svelte:window on:keypress={handleEnter} />

<style>
	.add-todo {
		display: flex;
		margin-bottom: 40px;
	}
</style>
