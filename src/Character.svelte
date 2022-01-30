<script>
	export let todayTaskCountShow, todos;

	let lv0 = ["Let's do this", "Let's get this party started", 'You can do it!'];
	let lv1 = ['One done', 'Good start'];
	let lv2 = ['Two done', 'Keep going'];
	let lv3 = ['Three done', "You're on a roll!"];
	let lv4 = ['One done', 'Good start'];
	let lv5 = ['Two done', 'Keep going'];
	let lv6 = ['Three done', "You're on a roll!"];
	let lv7 = ['One done', 'Good start'];
	let lv8 = ['Two done', 'Keep going'];
	let lv9 = ['Three done', "You're on a roll!"];
	let lv10 = ['One done', 'Good start'];

	const randomize = (arr) => arr[Math.floor(Math.random() * arr.length)];
	// console.log(randomize(lv1));
</script>

<p>This is today task {todayTaskCountShow}</p>
<div class="bubble">
	{#if todayTaskCountShow === 0}
		<p>{randomize(lv0)}</p>
	{:else if todayTaskCountShow === 1}
		<p>{randomize(lv1)}</p>
	{:else if todayTaskCountShow === 2}
		<p>{randomize(lv2)}</p>
	{:else if todayTaskCountShow === 3}
		<p>{randomize(lv3)}</p>
	{:else if todayTaskCountShow === 4}
		<p>{randomize(lv4)}</p>
	{:else if todayTaskCountShow === 5}
		<p>{randomize(lv5)}</p>
	{:else if todayTaskCountShow === 6}
		<p>{randomize(lv6)}</p>
	{:else if todayTaskCountShow === 7}
		<p>{randomize(lv7)}</p>
	{:else if todayTaskCountShow === 8}
		<p>{randomize(lv8)}</p>
	{:else if todayTaskCountShow === 9}
		<p>{randomize(lv9)}</p>
	{:else if todayTaskCountShow > 9}
		<p>{randomize(lv10)}</p>
	{:else}
		<p>no condition</p>
	{/if}
</div>

<style>
	.bubble {
		max-width: 260px;
		background-color: #fff;
		margin: 5px;
		padding: 20px;
		border-radius: 300px;
		font-family: 'Schoolbell', Arial, Helvetica, sans-serif;
		font-size: 18px;
		display: flex;
		justify-content: center;
		align-items: center;
	}
</style>
