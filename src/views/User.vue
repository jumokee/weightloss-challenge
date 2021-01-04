<template>
	<div class="user">
		<img class="profile-image" :src="require(`@/assets/${user.name}.png`)" />
		<div class="entries-table">
			<div v-if="userEntries.length">
				<div class="entry" v-for="entry in userEntries">
					<span>{{ entry.date | moment('dddd, MMM D, YYYY ') }}</span>
					<span>{{ entry.weight }} lbs.</span>
				</div>
			</div>
			<div class="no-entries" v-else>No Entries Found</div>
		</div>
		<div class="new-entry">
			<label>New Entry</label>
			<div class="form-inline">
				<input type="number" placeholder="Enter Weight" v-model="weightEntry" />
				<button @click="submitWeight" id="submitWeight" class="btn btn-primary">Submit</button>
			</div>
		</div>
	</div>
</template>
<script>
	export default {
		data() {
			return {
				API_URL: 'https://5ff379b428c3980017b195bc.mockapi.io',
				user: this.$route.query,
				userEntries: [],
				allEntries: [],
				weightEntry: 0,
			};
		},
		methods: {
			getUserEntries: async function() {
				const id = this.user.id;
				let res = await fetch(this.API_URL + '/entries/');
				let allEntries = await res.json();
				let userEntries = allEntries.filter((entry) => entry.userId == id);
				this.userEntries = userEntries;
			},
			submitWeight: async function(form) {
				let weight = this.weightEntry;
				let date = Date.now();
				let submission = {
					weight: weight,
					date: date,
					userId: this.user.id,
				};
				let result = await fetch(this.API_URL + '/entries/', {
					headers: {
						'Content-Type': 'application/json',
					},
					method: 'POST',
					body: JSON.stringify(submission),
				});
				let response = await result.json();
				this.userEntries.push(response);
			},
		},
		mounted() {
			this.getUserEntries();
		},
	};
</script>
<style lang="scss" scoped>
	.user {
		width: 80vw;
		min-height: calc(80vw * 1.2);
		display: flex;
		flex-direction: column;
		justify-content: space-around;
		align-items: center;
		box-shadow: 5px 5px 5px $prussian-blue;
		background-color: $powder-blue;
		.profile-image {
			border-radius: 50%;
			width: 20%;
		}
		.entries-table {
			width: 90%;
		}
		.entry {
			display: flex;
			justify-content: space-between;
			font-weight: bold;
			border: 1px solid $prussian-blue;
			span:first-of-type {
				border-right: 1px solid $prussian-blue;
			}
		}
		.no-entries {
			color: $imperial-red;
			font-weight: bold;
			text-shadow: 1px 1px 3px $celadon-blue;
		}
		label {
			display: block;
			text-align: left;
			margin: 0;
		}
		input {
			height: 2rem;
			border-radius: 6px 0 0 6px;
			border-right: 0;
		}
		button {
			height: 2rem;
			border-radius: 0 6px 6px 0;
			background-color: $celadon-blue;
			border: none;
		}
	}
	@media screen and (min-width: 480px) {
		.user {
			width: 400px;
			min-height: 0;
			height: 400px;
		}
	}
</style>
