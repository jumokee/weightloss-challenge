<template>
	<div id="loginComponent">
		<div v-show="isLoading">
			<SimpleLoader />
		</div>
		<div v-show="!isLoading">
			<div class="title" v-if="isRegistration">Register</div>
			<div class="title" v-else>Login</div>
			<div id="formio"></div>
			<div class="login-links">
				<a @click="renderLogin">Login</a> |
				<a @click="renderRegisterForm">Register</a>
			</div>
		</div>
	</div>
</template>

<script>
	import { Formio } from 'formiojs';
	import SimpleLoader from '@/components/SimpleLoader.vue';
	export default {
		name: 'LoginComponent',
		components: { SimpleLoader },
		data() {
			return {
				API_URL: 'https://gyhyzjexzapirnz.form.io',
				isRegistration: false,
				isLoading: true,
			};
		},
		methods: {
			renderLogin: function() {
				let _this = this;
				_this.isLoading = true;
				this.isRegistration = false;
				Formio.createForm(document.querySelector('#formio'), this.API_URL + '/user/login', {
					hooks: {
						beforeSubmit: function() {
							return;
						},
					},
				}).then(function(form) {
					setTimeout(() => {
						_this.isLoading = false;
					}, 1000);
					form.nosubmit = true;
					let button = document.querySelector('button[name="data[submit]"]');
					form.on('change', function() {
						let data = form.submission.data;
						if (_this.validateForm(data)) {
							button.removeAttribute('disabled');
						} else {
							button.setAttribute('disabled', true);
						}
					});
					button.addEventListener('click', function(e) {
						let body = {
							data: {
								email: form.submission.data.email,
								password: form.submission.data.password,
							},
						};
						var myHeaders = new Headers();
						myHeaders.append('Content-Type', 'application/json');
						var requestOptions = {
							method: 'POST',
							headers: myHeaders,
							body: JSON.stringify(body),
							redirect: 'follow',
							mode: 'cors',
						};

						fetch(`${_this.API_URL}/user/login/`, requestOptions)
							.then((response) => {
								debugger;
								console.log(response.headers);
								return response.text();
							})
							.then((result) => console.log('result', result))
							.catch((error) => console.log('error', error));
					});
				});
				_this.isLoading = false;
			},
			renderRegisterForm: function() {
				let _this = this;
				_this.isLoading = true;
				_this.isRegistration = true;
				Formio.createForm(document.querySelector('#formio'), this.API_URL + '/user/register', {
					hooks: {
						beforeSubmit: function() {
							return;
						},
					},
				}).then(function(form) {
					setTimeout(() => {
						_this.isLoading = false;
					}, 1000);
					form.nosubmit = true;
					let button = document.querySelector('button[name="data[submit]"]');
					form.on('change', function() {
						let data = form.submission.data;
						if (_this.validateForm(data)) {
							button.removeAttribute('disabled');
						} else {
							button.setAttribute('disabled', true);
						}
					});
					button.addEventListener('click', function(e) {
						let body = {
							data: {
								email: form.submission.data.email,
								password: form.submission.data.password,
							},
						};
						var myHeaders = new Headers();
						myHeaders.append('Content-Type', 'application/json');
						var requestOptions = {
							method: 'POST',
							headers: myHeaders,
							body: JSON.stringify(body),
							redirect: 'follow',
							mode: 'cors',
						};

						fetch(`${_this.API_URL}/user/register/`, requestOptions)
							.then((response) => {
								debugger;
								console.log(response.headers);
								return response.text();
							})
							.then((result) => console.log('result', result))
							.catch((error) => console.log('error', error));
					});
				});
			},
			validateForm: function(data) {
				let hasError = [...document.querySelectorAll('.formio-errors')].some(
					(node) => node.innerText
				);
				return !hasError && data.email && data.password;
			},
		},
		mounted() {
			this.renderLogin();
		},
	};
</script>

<style lang="scss">
	.title {
		margin: 20px 0;
		font-size: 20px;
		font-weight: bold;
	}

	.login-links {
		color: $imperial-red;
		font-weight: bold;
		margin: 1rem 0;
		a {
			cursor: pointer;
			&:hover {
				text-decoration: underline;
			}
		}
	}
	.formio-form {
		input::placeholder {
			font-size: 0.8rem;
			color: $celadon-blue;
		}
		.col-form-label {
			float: left;
			padding: 0;
		}
		.form-control {
			border-radius: 0px;
		}
	}
</style>
