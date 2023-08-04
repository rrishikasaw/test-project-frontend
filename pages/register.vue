<script setup lang="ts">
import { useSnackStore } from '~/stores/snack';
let env = useRuntimeConfig().public;
let snack = useSnackStore();

let name = ref('');
let email = ref('');
let qualification = ref('');
let address = ref('');
let district = ref('');
let training = ref('');
let course = ref('');
let photo = ref();
let phone = ref('');
let password = ref('');
let city = ref('');
let pincode = ref('');
let dob = ref('');
let gender = ref('');
let country = ref('');
let state = ref('');
let branch = ref('');
let form = ref();

let nameRules = [(v) => !!v || 'name is required'];
let emailRules = [(v) => !!v || 'email is required', (v) => /^\S+@\S+\.\S+$/.test(v) || 'email is invalid'];
let passwordRules = [(v) => !!v || 'password is required'];
let dobRules = [(v) => !!v || 'dob is required'];
let phoneRules = [(v) => !!v || 'phone is required'];
let genderRules = [(v) => !!v || 'gender is required'];
let qualificationRules = [(v) => !!v || 'qualification is required'];
let countryRules = [(v) => !!v || 'country is required'];
let addressRules = [(v) => !!v || 'address is required'];
let cityRules = [(v) => !!v || 'city is required'];
let stateRules = [(v) => !!v || 'state is required'];
let pinRules = [(v) => !!v || 'pin is required'];
let branchRules = [(v) => !!v || 'branch is required'];
let districtRules = [(v) => !!v || 'district is required'];
let courseRules = [(v) => !!v || 'course is required'];

onMounted(() => {
	let token = localStorage.getItem('token');
	if (token) {
		return navigateTo('/');
	}
});

async function add() {
	// form validation
	const { valid } = await form.value.validate();
	if (!valid) {
		return snack.error('Please fill all data correctly');
	}
	let formData = new FormData();
	formData.append('name', name.value);
	formData.append('email', email.value);
	formData.append('qualification', qualification.value);
	formData.append('address', address.value);
	formData.append('district', district.value);
	formData.append('training', training.value);
	formData.append('course', course.value);
	formData.append('photo', photo.value.files[0]);
	formData.append('phone', phone.value);
	formData.append('password', password.value);
	formData.append('city', city.value);
	formData.append('pincode', pincode.value);
	formData.append('dob', dob.value);
	formData.append('gender', gender.value);
	formData.append('country', country.value);
	formData.append('state', state.value);
	formData.append('branch', branch.value);

	let res = await fetch(`${env.backend}/users/register`, {
		method: 'POST',
		body: formData,
	});

	let json = await res.json();
	console.log(json);
	if (!res.ok) {
		return snack.error(json.message);
	}
	snack.success('Register successful');
	navigateTo('/login');
}
</script>

<template>
	<v-container>
		<v-form ref="form">
			<div class="d-flex justify-end mb-5">
				<v-btn @click="navigateTo('/login')" class="me-2" rounded="pill" color="blue">Login</v-btn>
				<v-btn @click="navigateTo('/register')" class="" rounded="pill" color="green">SignUp</v-btn>
			</div>

			<h3 class="ml-16">Registration</h3>
			<p class="line"></p>
			<h5 class="ml-16 mb-10">Personal Details</h5>
			<div class="row justify-evenly">
				<div class="col-12 col-lg-4">
					<p>Full Name</p>
					<v-text-field
						:rules="nameRules"
						label="Enter your name"
						v-model="name"
						class="input"
						variant="outlined"
					></v-text-field>

					<p>Email</p>
					<v-text-field :rules="emailRules" label="Enter your Email" v-model="email" variant="outlined"></v-text-field>

					<p>Qualification</p>
					<v-text-field
						:rules="qualificationRules"
						label="Enter your qualification"
						v-model="qualification"
						variant="outlined"
					></v-text-field>

					<p>Full Address</p>
					<v-textarea :rules="addressRules" label="Enter Address" v-model="address" variant="outlined"></v-textarea>

					<p>District</p>
					<v-text-field
						:rules="districtRules"
						label="Enter District"
						v-model="district"
						variant="outlined"
					></v-text-field>
				</div>

				<div class="col-12 col-lg-4">
					<p>Profile Picture</p>
					<v-text-field type="file" ref="photo" class="input" variant="outlined"></v-text-field>

					<p>Mobile Number</p>
					<v-text-field
						:rules="phoneRules"
						label="Enter mobile number"
						v-model="phone"
						variant="outlined"
					></v-text-field>

					<p>Password</p>
					<v-text-field
						type="password"
						:rules="passwordRules"
						label="Enter your password"
						v-model="password"
						variant="outlined"
					></v-text-field>

					<p>City/Town</p>
					<v-text-field :rules="cityRules" label="Enter City" v-model="city" variant="outlined"></v-text-field>

					<p>Pincode</p>
					<v-text-field :rules="pinRules" label="Enter Pincode" v-model="pincode" variant="outlined"></v-text-field>

					<p>Training Type</p>
					<v-text-field label="Enter training" v-model="training" variant="outlined"></v-text-field>
				</div>

				<div class="col-12 col-lg-4">
					<p>Date of Birth</p>
					<v-text-field :rules="dobRules" class="input" v-model="dob" variant="outlined" type="date"></v-text-field>

					<p>Gender</p>
					<v-select
						:rules="genderRules"
						label="Select your gender"
						v-model="gender"
						:items="['Male', 'Female']"
					></v-select>

					<p>Country</p>
					<v-select
						label="Select your Country"
						:rules="countryRules"
						v-model="country"
						:items="['India', 'US', 'Canada', 'Germany', 'China', 'France']"
					></v-select>

					<p>State</p>
					<v-text-field
						:rules="stateRules"
						label="Enter your State"
						v-model="state"
						class="input"
						variant="outlined"
					></v-text-field>

					<p>Branch</p>
					<v-text-field
						:rules="branchRules"
						label="Enter your Branch"
						v-model="branch"
						class="input"
						variant="outlined"
					></v-text-field>

					<p>Course Details</p>
					<v-text-field label="Enter course" :rules="courseRules" v-model="course" variant="outlined"></v-text-field>
				</div>
			</div>

			<v-btn @click="add()" class="button d-block mx-auto my-5" size="large"> Submit </v-btn>
		</v-form>
	</v-container>
</template>

<style scoped>
h3 {
	color: rgb(45, 220, 144);
}

p {
	color: rgb(45, 220, 144);
	font-size: 15px;
	font-weight: bold;
}

.button {
	background-color: rgb(45, 220, 144);
	color: white;
	padding-inline: 60px;
}

.line {
	width: 30px;
	border-top: 5px solid black;
	margin-left: 60px;
	margin-top: 20px;
}
</style>
