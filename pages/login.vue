<script setup>
import { useSnackStore } from '~/stores/snack';

let env = useRuntimeConfig().public;
let snack = useSnackStore();
let showPassword = ref(false);
let form = ref();
let passwordType = computed(() => (showPassword.value ? 'text' : 'password'));

let email = ref('');
let password = ref('');

let emailRules = [(v) => !!v || 'email is required', (v) => /^\S+@\S+\.\S+$/.test(v) || 'email is invalid'];
let passwordRules = [(v) => !!v || 'password is required'];

onMounted(() => {
	let token = localStorage.getItem('token');
	if (token) {
		return navigateTo('/');
	}
});

async function submit() {
	const { valid } = await form.value.validate();
	if (!valid) {
		return
	}

	let res = await fetch(`${env.backend}/users/login`, {
		headers: {
			'Content-Type': 'application/json',
		},
		body: JSON.stringify({
			email: email.value,
			password: password.value,
		}),
		method: 'POST',
	});

	let json = await res.json();
	console.log(json);
	if (!res.ok) {
		return snack.error(json.message);
	}
	snack.success('login successful');

	localStorage.setItem('token', json.token);
	navigateTo('/');
}
</script>

<template>
	<div class="photo">
		<v-container class="mt-0">
			<div class="d-flex justify-end mb-5">
				<v-btn @click="navigateTo('/login')" class="me-2" rounded="pill" color="blue">Login</v-btn>
				<v-btn @click="navigateTo('/register')" class="" rounded="pill" color="green">SignUp</v-btn>
			</div>

			<div class="card mx-auto">
				<h3 class="text-center mb-10">Welcome To BeanGate</h3>
				<v-form ref="form">
					<v-text-field :rules="emailRules" v-model="email" label="mail@example.com" density="compact" variant="solo"
						rounded="pill"></v-text-field>

					<v-text-field :rules="passwordRules" label="password" v-model="password" density="compact" variant="solo"
						:type="passwordType" rounded="pill"></v-text-field>

					<div class="d-flex">
						<v-checkbox label="Show Password" hide-details v-model="showPassword"></v-checkbox>
						<p></p>
					</div>

					<div class="d-flex justify-between">
						<v-btn color="green" rounded="pill" @click="submit">Submit</v-btn>
						<!-- <p class="text-blue">Forget Password</p> -->
					</div>
				</v-form>
			</div>
		</v-container>
	</div>
</template>

<style scoped>
.photo {
	background-image: url('../assets/images/login-wallpaper.jpg');
	height: 100dvh;
}

.card {
	max-width: 500px;
	background-color: rgb(231, 249, 249);
	padding: 30px;
}

.button {
	background-color: rgb(10, 190, 10);
	border: 0;
	padding-inline: 20px;
	padding-top: 10px;
	padding-bottom: 10px;
	border-radius: 20px;
	color: white;
}
</style>
