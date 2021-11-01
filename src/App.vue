<template>
	<navigation />
	<main>
		<router-view />
	</main>
</template>

<script>
import Navigation from '@/components/Navigation.vue';
export default {
	components: {
		Navigation,
	},
};
</script>

<style lang="scss">
html {
	scroll-behavior: smooth;
	white-space: normal;
}

* {
	box-sizing: border-box;
	font-size: 15px;
	margin: 0;
	overflow-x: hidden;
	color: #fff;
}

body {
	background-image: linear-gradient(
		to bottom,
		#1a1a2e,
		#1c203a,
		#1c2646,
		#182d53,
		#0f3460
	);
	background-repeat: no-repeat;
	background-position: center;
	min-height: 100%;
}

h1 {
	margin: 0;
	font-size: 2rem;
}

h2 {
	margin: 0;
	font-size: 1.5rem;
}

h3 {
	margin: 0;
	font-size: 1.25rem;
}

h4 {
	margin: 0;
	font-size: 1rem;
}

h5 {
	margin: 0;
	font-size: 0.85rem;
}

h6 {
	margin: 0;
	font-size: 0.7rem;
}

img {
	max-width: 100%;
	height: auto;
}

p {
	line-height: 1.6;
}

a {
	text-decoration: none;
	color: #fff;
}

.container {
	max-width: 1200px;
	margin: 0 auto;
}
</style>
