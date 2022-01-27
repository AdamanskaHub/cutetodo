<script>
	export let todo, updateTodo, deleteTodo, today;
	import { blur, crossfade, draw, fade, fly, scale, slide } from 'svelte/transition';
	import Bomb from '../../static/bomb.svelte';
	let isDeleted = false;
	console.log(todo.task);
</script>

<div class={isDeleted ? 'todos deletor' : 'todos'} class:done={todo.isComplete}>
	<div class="control-group">
		<label class="control control-checkbox">
			{todo.task}
			<input
				type="checkbox"
				checked={todo.isComplete}
				disabled={todo.isComplete}
				on:change={(e) => {
					todo.isComplete = e.currentTarget.checked;
					updateTodo(todo);
				}}
			/>
			<div class="control_indicator" />
		</label>
	</div>
	<div class="todos-right">
		<p>{today == todo.date ? 'today' : 'overdue'}</p>
		<button
			class="btn-delete"
			on:click={() => {
				deleteTodo(todo);
			}}
		>
			<Bomb stylor={'#A5DFD3'} />
		</button>
	</div>
</div>

<!-- <input
		type="text"
		value={todo.task}
		on:input={(e) => {
			todo.task = e.currentTarget.value;
			updateTodo(todo);
		}}
	/> -->

<!-- transition:slide={{ duration: 350 }} -->

<!-- transition:fly={{ x: 30 }} -->

<!-- if todo is complete adds call done -->

<!-- on:click={() => {
	(isDeleted = true),
		setTimeout(() => {
			deleteTodo(todo);
		}, 1000);
}} -->

<!-- .successfully-saved.hide-opacity {
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
	} -->
<style>
	.todos {
		display: flex;
		align-items: center;
		justify-content: space-between;
		width: 360px;
		height: 40px;
		padding: 0 10px;
		margin-bottom: 10px;
		background: #ffffff;
		color: #333;
		box-shadow: 0px 2px 6px rgba(0, 0, 0, 0.25);
		border-radius: 10px;
		transition: all 1s ease-in-out;
	}
	.todos-right {
		display: flex;
		align-items: center;
	}
	.checkbox {
		width: 1.3em;
		height: 1.3em;
		background-color: white;
		border-radius: 50%;
		vertical-align: middle;
		border: 1px solid #ddd;
		appearance: none;
		-webkit-appearance: none;
		outline: none;
		cursor: pointer;
	}
	.checkbox:checked {
		background: transparent;
	}
	.round {
		position: relative;
	}

	.round label {
		background-color: #fff;
		border: 1px solid #ccc;
		border-radius: 50%;
		cursor: pointer;
		height: 28px;
		left: 0;
		position: absolute;
		top: 0;
		width: 28px;
	}

	.round label:after {
		border: 2px solid #fff;
		border-top: none;
		border-right: none;
		content: '';
		height: 6px;
		left: 7px;
		opacity: 0;
		position: absolute;
		top: 8px;
		transform: rotate(-45deg);
		width: 12px;
	}

	.round input[type='checkbox'] {
		visibility: hidden;
	}

	.round input[type='checkbox']:checked + label {
		background-color: #66bb6a;
		border-color: #66bb6a;
	}

	.round input[type='checkbox']:checked + label:after {
		opacity: 1;
	}
	.done {
		opacity: 0.5;
	}
	/* .done input[type='text'] {
		text-decoration: line-through;
	} */

	.btn-delete {
		border: transparent;
		background: transparent;
	}
	.btn-delete > svg > path {
		fill: violet;
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

	.control {
		font-family: arial;
		display: block;
		position: relative;
		padding-left: 30px;
		margin-bottom: 5px;
		padding-top: 3px;
		cursor: pointer;
		font-size: 16px;
	}
	.control input {
		position: absolute;
		z-index: -1;
		opacity: 0;
	}
	.control_indicator {
		position: absolute;
		top: 2px;
		left: 0;
		height: 20px;
		width: 20px;
		background: #e6e6e6;
		border: 0px solid #000000;
		border-radius: 20px;
	}
	.control:hover input ~ .control_indicator,
	.control input:focus ~ .control_indicator {
		background: #cccccc;
	}

	.control input:checked ~ .control_indicator {
		background: #2aa1c0;
	}
	.control:hover input:not([disabled]):checked ~ .control_indicator,
	.control input:checked:focus ~ .control_indicator {
		background: #0e6647;
	}
	.control input:disabled ~ .control_indicator {
		background: #e6e6e6;
		opacity: 0.6;
		pointer-events: none;
	}
	.control_indicator:after {
		box-sizing: unset;
		content: '';
		position: absolute;
		display: none;
	}
	.control input:checked ~ .control_indicator:after {
		display: block;
	}
	.control-checkbox .control_indicator:after {
		left: 8px;
		top: 4px;
		width: 3px;
		height: 8px;
		border: solid #ffffff;
		border-width: 0 2px 2px 0;
		transform: rotate(45deg);
	}
	.control-checkbox input:disabled ~ .control_indicator:after {
		border-color: #7b7b7b;
	}
	.control-checkbox .control_indicator::before {
		content: '';
		display: block;
		position: absolute;
		left: 0;
		top: 0;
		width: 4.5rem;
		height: 4.5rem;
		margin-left: -1.3rem;
		margin-top: -1.3rem;
		background: #2aa1c0;
		border-radius: 3rem;
		opacity: 0.6;
		z-index: 99999;
		transform: scale(0);
	}
	.control-group > label {
		margin-bottom: 0;
		color: #333;
	}
	@keyframes s-ripple {
		0% {
			transform: scale(0);
		}
		20% {
			transform: scale(1);
		}
		100% {
			opacity: 0;
			transform: scale(1);
		}
	}
	@keyframes s-ripple-dup {
		0% {
			transform: scale(0);
		}
		30% {
			transform: scale(1);
		}
		60% {
			transform: scale(1);
		}
		100% {
			opacity: 0;
			transform: scale(1);
		}
	}
	.control-checkbox input + .control_indicator::before {
		animation: s-ripple 250ms ease-out;
	}
	.control-checkbox input:checked + .control_indicator::before {
		animation-name: s-ripple-dup;
	}
</style>
