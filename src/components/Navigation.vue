<template>
	<header>
		<nav>
			<router-link :to="{ name: 'Home' }">Movie<span>App</span> </router-link>
		</nav>
	</header>
</template>

<script>
export default {
	name: 'Navigation',
};
</script>

<style lang="scss" scoped>
header {
	nav {
		position: relative;
		display: flex;
		align-items: center;
		justify-content: center;
		background-color: #0f0f1a;
		height: 60px;

		a {
			font-size: 2rem;
			font-weight: bold;
			cursor: pointer;
			color: #fff;

			span {
				color: #1e68c2;
				font-size: 2rem;
			}
		}
	}
}
</style>
