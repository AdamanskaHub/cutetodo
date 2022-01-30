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

<p class="welcome">{isNewRegistration ? 'Welcome!' : 'Welcome back!'}</p>

<div class="main">
	<div class="left">
		<label for=""
			>Email
			<input type="email" placeholder="email address" bind:value={email} />
		</label>
		<div class="btn-grp">
			{#if isNewRegistration}
				<button on:click={signUp}>Sign up</button>
				<p class="little-link" on:click={() => (isNewRegistration = false)}>
					Already have an account?
				</p>
			{:else}
				<button on:click={logIn}>Log in</button>
				<p class="little-link" on:click={() => (isNewRegistration = true)}>No account yet?</p>
			{/if}
		</div>
	</div>
	<div class="right" />
</div>

<style>
	.welcome {
		text-align: center;
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
	.main {
		display: flex;
	}
	.left {
		margin-right: 10px;
	}
	.right {
		margin-left: 10px;
	}
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
