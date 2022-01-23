<script>
	import supabase from '$lib/db';
	import { onMount } from 'svelte';
	import Todo from '$lib/Todo.svelte';
	import Character from '../Character.svelte';
	import { user } from '$lib/stores';
	import { goto } from '$app/navigation';

	let newTask = '';
	let taskCount = 0;
	let todayTaskCountShow = 0;
	let today = new Date().toLocaleDateString();

	let todos = [''];
	onMount(async () => {
		getallTodos();
	});

	const getallTodos = async () => {
		try {
			let { data, error } = await supabase.from('todos').select('*');
			todos = data;
			// console.log(data);
			// data.sort((a, b) => a.id - b.id);
			console.log(data);
			reorder();
		} catch (err) {
			console.log(err);
		}
	};

	const reorder = async () => {
		try {
			let { data, error } = await supabase
				.from('todos')
				.select('*')
				.order('id', { ascending: false });
			console.log(data);
			todos = data;
		} catch (err) {
			console.log(err);
		}
	};

	const addTodo = async () => {
		try {
			const { data, error } = await supabase
				.from('todos')
				.insert([{ task: newTask, isComplete: false, user_id: $user.id, date: today }]);
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

			updateCookie();
		} catch (err) {
			console.log(err);
		}
	};

	const deleteTodo = async (todo) => {
		try {
			const { data, error } = await supabase.from('todos').delete().eq('id', todo.id);
			await getallTodos();
		} catch (err) {
			console.log(err);
		}
		console.table('finished delete' + todo);
	};

	const handleEnter = (e) => {
		if (e.key === 'Enter' && newTask !== '') {
			addTodo();
		}
	};

	const logout = async () => {
		let { error } = await supabase.auth.signOut();
		$user = false;
		goto('/login');
	};

	// ==== COOKIES =====

	onMount(() => {
		console.log('Mounting');
		createCookie();
	});

	function checkCookie(cookieName) {
		console.log('checking cookie');
		let name = cookieName + '=';
		let spli = document.cookie.split(';');
		for (var j = 0; j < spli.length; j++) {
			let char = spli[j];
			while (char.charAt(0) == ' ') {
				char = char.substring(1);
			}
			if (char.indexOf(name) == 0) {
				console.log(char.substring(name.length, char.length));
				return char.substring(name.length, char.length);
			}
		}
		console.log('empty');
		return '';
	}

	const updateCookie = () => {
		console.log('update cookie');
		var ttc = checkCookie('todayTaskCount');
		if (ttc != '') {
			let newCount = parseInt(ttc) + 1;
			document.cookie = 'todayTaskCount' + '=' + newCount + ';SameSite=Lax';
			console.log('increase ' + document.cookie);
			todayTaskCountShow = newCount;
		}
		//if ttc is null
		// else {
		// 	console.log('creating cookie'); //if ttc is not then add one
		// 	var date = new Date();
		// 	date.setTime(date.getTime() + 1 * 24 * 60 * 60 * 1000);
		// 	var expires = ';expires=' + date.toUTCString();
		// 	document.cookie = 'todayTaskCount' + '=' + 0 + expires + ';path=/' + ';SameSite=Lax';
		// 	console.log('created cookie ' + document.cookie);

		// 	//set cookie
		// 	if (ttc != '' && ttc != null) {
		// 		console.log('???');
		// 	}
		// }
	};
	const createCookie = () => {
		var ttc = checkCookie('todayTaskCount');
		if (ttc == '' || ttc == null) {
			console.log('creating cookie'); //if ttc is not then add one
			var date = new Date();
			date.setTime(date.getTime() + 1 * 24 * 60 * 60 * 1000);
			var expires = ';expires=' + date.toUTCString();
			document.cookie = 'todayTaskCount' + '=' + 0 + expires + ';path=/' + ';SameSite=Lax';
			console.log('created cookie ' + document.cookie);

			//set cookie
			if (ttc != '' && ttc != null) {
				console.log('???');
			}
		}
	};
</script>

<h3>Welcome {$user?.email ? $user.email : ''}</h3>

<p>today tasks count IS {todayTaskCountShow}</p>

<div class="add-todo">
	<input type="text" bind:value={newTask} />
	<button on:click={() => addTodo(newTask)}> Add task </button>
</div>

{#each todos as todo}
	<Todo {todo} {updateTodo} {deleteTodo} {today} />
{:else}
	<p>no tasks</p>
{/each}

{#if $user.email}
	<button class="little-link" on:click={logout}>Logout</button>
{/if}

<svelte:window on:keypress={handleEnter} />

<Character {taskCount} {todos} />

<style>
	.add-todo {
		display: flex;
		margin-bottom: 40px;
	}
	:global(.little-link) {
		background: none;
		border: none;
		text-decoration: underline;
		cursor: pointer;
	}
	.little-link {
		background: none;
		border: none;
		text-decoration: underline;
		cursor: pointer;
	}
</style>
