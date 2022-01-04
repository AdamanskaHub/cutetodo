<script>
	import { goto } from '$app/navigation';

	import supabase from '$lib/db';
	import { user } from '$lib/stores';

	let email = '';

	const signUp = async () => {
		let { user: userDetails, error } = await supabase.auth.signUp({
			email: email,
			password: 'CXMBZCQWXWEKlheMnOgY'
		});
		$user = userDetails;
		goto('/');
	};

	const logIn = async () => {
		let { user: userDetails, error } = await supabase.auth.signIn({
			email: email,
			password: 'CXMBZCQWXWEKlheMnOgY'
		});
		$user = userDetails;
		goto('/');
	};
</script>

<label for=""
	>Email
	<input type="email" placeholder="email address" bind:value={email} />
</label>
<div class="btn-grp">
	<button on:click={signUp}>Sign up</button>
	<button on:click={logIn}>Log in</button>
</div>

<style>
	.btn-grp {
		display: flex;
	}
</style>
