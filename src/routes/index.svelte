<script>
	import supabase from '$lib/db';
	import { onMount } from 'svelte';
	import Todo from '$lib/Todo.svelte';
	import Character from '../Character.svelte';
	import { user } from '$lib/stores';
	import { goto } from '$app/navigation';
	import Rocket from '../../static/rocket.svelte';

	let newTask = '';
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
			reorder();
			await getallTodos();
			reorder();

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
			reorder();

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
		// console.log('empty');
		return '';
	}

	const updateCookie = () => {
		console.log('update cookie');
		var ttc = checkCookie('todayTaskCount');
		if (ttc != '') {
			let newCount = parseInt(ttc) + 1;
			document.cookie = 'todayTaskCount' + '=' + newCount + ';SameSite=Lax';
			// console.log('increase ' + document.cookie);
			todayTaskCountShow = newCount;
			console.log(todayTaskCountShow, newCount);
		}
		//if ttc is null
		// else {
		// 	//set cookie
		// 	if (ttc != '' && ttc != null) {
		// 		console.log('???');
		// 	}
		// }
	};
	const createCookie = () => {
		var ttc = checkCookie('todayTaskCount');
		if (ttc == '' || ttc == null) {
			// console.log('creating cookie'); //if ttc is not then add one
			var date = new Date();
			// date = date.setTime(date.getTime() + 1 * 24 * 60 * 60 * 1000);
			date.setDate(date.getDate() + 1);
			date.setHours(2);
			date.setMinutes(0);
			console.log('date -- ' + date);
			var expires = ';expires=' + date.toUTCString();
			document.cookie = 'todayTaskCount' + '=' + 0 + expires + ';path=/' + ';SameSite=Lax';
			todayTaskCountShow = 0;
			// console.log('created cookie ' + document.cookie);
			//set cookie
			if (ttc != '' && ttc != null) {
				console.log('???');
			}
		}
	};
</script>

<div class="main">
	<div class="top">
		<div class="login">
			<!-- <span>{$user?.email ? $user.email : ''}</span> -->
			{#if $user.email}
				<button class="little-link" on:click={logout}>Logout</button>
			{/if}
		</div>

		<p class="combo">{todayTaskCountShow === 0 ? '' : '+' + todayTaskCountShow + ' COMBO'}</p>
	</div>

	<div class="left-right">
		<div class="left">
			<div class="add-todo">
				<input class="add-input" type="text" bind:value={newTask} placeholder="Add a task" />
				<button class="add-btn" on:click={() => addTodo(newTask)}>
					<Rocket />
				</button>
			</div>

			{#each todos as todo}
				<Todo {todo} {updateTodo} {deleteTodo} {today} />
			{:else}
				<p>Add a task</p>
			{/each}
		</div>

		<div class="right">
			<Character {todayTaskCountShow} {todos} />
		</div>
	</div>
</div>
<svelte:window on:keypress={handleEnter} />

<style>
	:root {
		--color1: #74b29d;
		--color2: #a5dfd3;
		--color3: #eefed4;
		--color4: #f8e396;
		--color5: #ee7ea0;
		font-family: 'Poppins', Arial, Helvetica, sans-serif;
	}
	:global(*, *:before, *:after) {
		-webkit-box-sizing: inherit;
		-moz-box-sizing: inherit;
		box-sizing: inherit;
	}
	:global(body) {
		min-height: 100vh;
		background: var(--color2);
		background-image: linear-gradient(var(--color1), var(--color2));
		box-sizing: border-box;
	}

	:global(.little-link) {
		background: none;
		border: none;
		text-decoration: underline;
		cursor: pointer;
	}
	.main {
		display: flex;
		flex-direction: column;
		align-items: center;
	}
	.top {
	}
	.top > p {
		font-family: 'Kalam', Arial, Helvetica, sans-serif;
		font-weight: bold;
		font-size: 64px;
		margin: 0;
		-webkit-text-stroke: 2px #fff;
		background-color: var(--color5);
		background-image: linear-gradient(35deg, var(--color4), var(--color5));
		background-size: 100%;
		background-repeat: repeat;
		-webkit-background-clip: text;
		-webkit-text-fill-color: transparent;
		-moz-background-clip: text;
		-moz-text-fill-color: transparent;
	}
	.combo {
		height: 110px;
	}
	.login {
		display: flex;
		margin-bottom: 40px;
		font-size: 12px;
	}
	.left-right {
		display: flex;
	}
	.left {
		margin-right: 10px;
	}
	.right {
		margin-left: 10px;
	}
	.little-link {
		background: none;
		border: none;
		text-decoration: underline;
		cursor: pointer;
	}
	.add-todo {
		display: flex;
		align-items: center;
		margin-bottom: 20px;
	}
	.add-input {
		border: 2px solid #fff;
		border-radius: 10px;
		background: rgba(255, 255, 255, 0.4);
		height: 50px;
		width: 360px;
		padding: 10px;
		font-size: 24px;
		color: #fff;
	}
	.add-btn {
		background-color: var(--color5);
		height: 46px;
		width: 48px;
		border: none;
		border-radius: 10px;
		transform: translateX(-50px);
		transition: all 0.3s ease-in-out;
	}
	.add-btn:hover {
		background-color: var(--color1);
	}
</style>
