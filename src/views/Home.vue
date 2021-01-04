<template>
	<div class="home">
		<LoginComponent />
	</div>
</template>

<script>
	// @ is an alias to /src
	import LoginComponent from '@/components/LoginComponent.vue';

	export default {
		name: 'Home',
		components: {
			LoginComponent,
		},
	};
</script>

<style lang="scss" scoped>
	.home {
		width: 80vw;
		min-height: calc(80vw * 1.2);
		display: flex;
		justify-content: center;
		align-items: center;
		box-shadow: 5px 5px 5px $prussian-blue;
		background-color: $powder-blue;
	}
	@media screen and (min-width: 480px) {
		.home {
			width: 400px;
			min-height: 0;
			height: 400px;
		}
	}
</style>
