<template>
	<div id="loginComponent">
		<div class="select-box" @click="showOptions">
			<div class="select">Who are you?</div>
			<div class="users">
				<div class="user" :data-user-id="user.id" v-for="user in users">
					<router-link
						:to="{ name: 'user', query: { name: user.name, id: user.id } }"
						@click.native="$event.stopImmediatePropagation()"
					>
						<img :src="require(`@/assets/${user.image}`)" />
						<span>{{ user.name }}</span>
					</router-link>
				</div>
			</div>
		</div>
	</div>
</template>

<script>
	export default {
		data() {
			return {
				API_URL: 'https://5ff379b428c3980017b195bc.mockapi.io',
				users: [],
			};
		},
		methods: {
			showOptions: function(e) {
				document.querySelector('.select-box').classList.toggle('open');
			},
			getUsers: async function() {
				let res = await fetch(this.API_URL + '/users/');
				let users = await res.json();
				users.map((user) => (user.image = `${user.name}.png`));
				this.users = users;
			},
		},
		mounted() {
			this.getUsers();
		},
	};
</script>

<style lang="scss">
	#loginComponent {
		width: 100%;
		margin: auto;
	}
	.select-box {
		cursor: pointer;
		background-color: white;
		width: 50%;
		margin: auto;
		position: relative;
	}
	.select {
		border: 1px solid black;
		&:after {
			position: absolute;
			right: 5px;
			content: '🔻';
		}
	}
	.users {
		height: 0;
		visibility: hidden;
		opacity: 0;
		transition: visibility 0s, opacity 0.5s linear;
		.user {
			padding: 3px 15px;
			border: 1px solid black;
			border-top: none;
			display: flex;
			justify-content: center;
			align-items: center;
			a {
				width: 100%;
				display: flex;
				justify-content: space-around;
				align-items: center;
			}
			img {
				width: 50px;
				border-radius: 50%;
				border: 1px solid black;
			}
			span {
				text-transform: capitalize;
			}
		}
	}
	.open .users {
		height: auto;
		visibility: visible;
		opacity: 1;
	}
</style>
