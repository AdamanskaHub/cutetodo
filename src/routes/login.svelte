<script>
	import { goto } from '$app/navigation';

	import supabase from '$lib/db';
	import { user } from '$lib/stores';

	let email = '';
	let isNewRegistration = false;

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
	{#if isNewRegistration}
		<button on:click={signUp}>Sign up</button>
		<p class="little-link" on:click={() => (isNewRegistration = false)}>Already have an account?</p>
	{:else}
		<button on:click={logIn}>Log in</button>
		<p class="little-link" on:click={() => (isNewRegistration = true)}>No account yet?</p>
	{/if}
</div>

<style>
	.btn-grp {
		display: flex;
	}
	.little-link {
		background: none;
		border: none;
		text-decoration: underline;
		cursor: pointer;
	}
</style>
