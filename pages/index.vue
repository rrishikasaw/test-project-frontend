<script setup>


import { useSnackStore } from '~/stores/snack';
let env = useRuntimeConfig().public
let snack = useSnackStore()

let name = ref('')
let branch = ref('')
let training = ref('')
let email = ref('')
let phone = ref('')
let address = ref('')
let photo = ref('')


let photoUrl = computed(() => `${env.backendUploads}/${photo.value}`)


function logout() {
	localStorage.removeItem('token');
	navigateTo('/login');
}

onMounted(async () => {
	let token = localStorage.getItem("token")

	if (!token) {
		return navigateTo('/login')
	}

	let res = await fetch(`${env.backend}/users/profile`, {
		headers: {
			authorization: `Bearer ${token}`,
		},
	})

	if (!res.ok) {
		return snack.error(json.message)
	}

	let json = await res.json()
	name.value = json.name;
	branch.value = json.branch;
	training.value = json.training;
	email.value = json.email;
	phone.value = json.phone;
	address.value = json.address
	photo.value = json.photo

})



</script>

<template>
	<v-container>


		<div class="row justify-content-evenly">


			<div class="first-card col-12 col-lg-3 mb-3">

				<h5 class="text-center font-weight-bold mt-2">
					<v-avatar color="info" size="x-large" class="mr-3" v-if="photo">
						<img :src="photoUrl" alt="">
					</v-avatar>
					Admin
					<v-btn title="Logout" icon="mdi-power-standby" density="compact" color="red" @click="logout"></v-btn>
				</h5>
				<h2 class="text-center green-text">{{ name }}--BGA032</h2>
				<p class="text-center">@BeanGate Academy</p>

				<div class="d-flex justify-center mb-4">
					<v-btn class="me-2" color="green" size="small" variant="outlined">Message</v-btn>
					<v-btn color="green" size="small" variant="outlined">Edit Profile</v-btn>
				</div>
			</div>






			<div class="second-card col-12 col-lg-8 mb-3">
				<h2 class="green-text mb-6 mt-2">
					<Icon name="mdi:person-card-details" class="fz-10 me-2" />
					Student Details
				</h2>
				<div class="d-flex">
					<p class="title">Full Name</p>
					<p class="ml-16">{{ name }}</p>
				</div>
				<hr />

				<div class="d-flex">
					<p class="title">Branch</p>
					<p class="ml-16">{{ branch }}</p>
				</div>
				<hr />

				<div class="d-flex">
					<p class="title">Training</p>
					<p class="ml-16">{{ training }}</p>
				</div>
				<hr />

				<div class="d-flex">
					<p class="title">User Code</p>
					<p class="ml-16">BGA032</p>
				</div>
				<hr />

				<div class="d-flex">
					<p class="title">Email</p>
					<p class="ml-16">{{ email }}</p>
				</div>
				<hr />

				<div class="d-flex">
					<p class="title">Phone</p>
					<p class="ml-16">{{ phone }}</p>
				</div>
				<hr />

				<div class="d-flex">
					<p class="title">Address</p>
					<p class="ml-16">{{ address }}</p>
				</div>
			</div>
		</div>

		<div class="row justify-content-evenly">
			<div class="third-card mb-3 col-12 col-lg-3">
				<h2 class="green-text mb-6 mt-2">
					<Icon name="fa-solid:rupee-sign" class="fz-10 me-2" />
					Fees Structure
				</h2>
				<v-btn color="red" class="mb-4" block>UnPaid</v-btn>
			</div>

			<div class="forth-card col-12 col-lg-8">
				<h2 class="green-text mb-6 mt-2">
					<Icon name="fa6-solid:book" class="fz-10 me-2" />
					Course Details
				</h2>
				<hr />

				<v-table height="300px">
					<thead>
						<tr>
							<th class="text-left">S. no.</th>
							<th class="text-left">Course Name</th>
							<th class="text-left">Faculty</th>
							<th class="text-left">Duration</th>
							<th class="text-left">Class Time</th>
						</tr>
					</thead>
					<tbody>
						<tr>
							<td>1.</td>
							<td>Java Full Stack</td>
							<td>Pankaj Sir</td>
							<td>6 months</td>
							<td>2023-03-07 16:00:00</td>
						</tr>
					</tbody>
				</v-table>
			</div>
		</div>

		<div class="my-3 row justify-content-evenly">
			<div class="first-card col-12 col-lg-3 mb-4 py-4">
				<h3 class="green-text font-italic mb-6">Frontend Course</h3>
				<v-file-input label="Upload Assignment" density="compact"></v-file-input>
				<v-btn color="green">submit</v-btn>
			</div>
			<div class="first-card col-12 col-lg-3 mb-4 py-4">
				<h3 class="green-text font-italic mb-6">Backend Course</h3>
				<v-file-input label="Upload Assignment" density="compact"></v-file-input>
				<v-btn color="green">submit</v-btn>
			</div>
			<div class="first-card col-12 col-lg-3 mb-4 py-4">
				<h3 class="green-text font-italic mb-6">Database</h3>
				<v-file-input label="Upload Assignment" density="compact"></v-file-input>
				<v-btn color="green">submit</v-btn>
			</div>
		</div>
	</v-container>
</template>

<style scoped>
.first-card {
	border: 2px solid rgb(9, 182, 9);
	max-height: 200px;
}

.green-text {
	font-size: 22px;
	font-weight: bold;
	color: rgb(9, 182, 9);
}

.second-card {
	border: 2px solid rgb(9, 182, 9);
}

.title {
	color: rgb(9, 182, 9);
	font-weight: bold;
	width: 110px;
}

.third-card {
	border: 2px solid rgb(9, 182, 9);
	max-height: 130px;
}

.forth-card {
	border: 2px solid rgb(9, 182, 9);
}

.fees {
	font-size: 30px;
	font-weight: bold;
	color: rgb(9, 182, 9);
}
</style>
