<script>
	import supabase from '$lib/db';
	import { onMount } from 'svelte';
	import Todo from '$lib/Todo.svelte';

	let todos = ['hfeiwh'];
	onMount(async () => {
		let { data, error } = await supabase.from('todos').select('*');
		todos = data;
	});
	const updateTodo = (todo) => {
		// try {
		// 	const { data, error } = await supabase
		// 		.from('todos')
		// 		.update({ task: todo.task })
		// 		.eq('id', 'todo.id');
		// } catch (err) {
		// 	console.log(err);
		// }
		console.table(todo);
	};
</script>

{#each todos as todo}
	<Todo {todo} {updateTodo} />
{:else}
	<p>no tasks</p>
{/each}
