<script>
	import { goto } from '$app/navigation';
	import {
		getAuth,
		signInWithEmailAndPassword,
		createUserWithEmailAndPassword
	} from 'firebase/auth';

	export let title = '';

	const auth = getAuth();
	let errorCode = '';
	let errorMessage = '';

	let email = '';
	let password = '';
	async function login() {
		if (title == 'Login') {
			await signInWithEmailAndPassword(auth, email, password)
				.then((userCredential) => {
					// Signed in
					const user = userCredential.user;
					localStorage.setItem('uid', user.uid);
					localStorage.setItem('isLoggedIn', 'true');
					goto('/');
				})
				.catch((error) => {
					errorCode = error.code;
					errorMessage = error.message;
				});
		} else {
			console.log(email, password);
			await createUserWithEmailAndPassword(auth, email, password)
				.then((userCredential) => {
					const user = userCredential.user;
					console.log(user);
					goto('/');
				})
				.catch((error) => {
					errorCode = error.code;
					errorMessage = error.message;
				});
		}
	}
</script>

{errorCode}
{errorMessage}

<div class="login">
	<div class="card">
		<div class="card-body login-form">
			<h5 class="card-title">{title}</h5>
			<div class="mb-3">
				<label for="emailInput" class="form-label">Email address</label>
				<input
					type="email"
					class="form-control"
					id="emailInput"
					placeholder="Email Address"
					bind:value={email}
				/>
			</div>
			<div class="mb-3">
				<label for="passInput" class="form-label">Password</label>
				<input
					type="password"
					bind:value={password}
					class="form-control"
					id="passInput"
					placeholder="Password"
				/>
			</div>
			<button on:click={login} class="btn btn-primary">Submit</button>
			{#if title == 'Login'}
				<p class="float-end mt-3">
					Not a user? <a href="/signup" class="card-link">Sign Up</a>
				</p>
			{/if}
		</div>
	</div>
</div>

<style>
	.card {
		width: 50%;
		margin: 0 auto;
	}
	.login {
		margin-top: 50px;
		margin-bottom: 50px;
	}
	.login-form {
		width: 60%;
		margin: 0 auto;
	}
	@media only screen and (min-device-width: 320px) and (max-device-width: 480px) {
		.login-form {
			width: 90%;
		}
		.card {
			width: 90%;
		}
	}
</style>