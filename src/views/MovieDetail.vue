<template>
	<div class="movie-detail container">
		<h2>{{ movie.Title }}</h2>
		<div class="container-flex">
			<img :src="movie.Poster" :alt="movie.Title + 'Poster'" />
			<div class="detail">
				<p class="plot">
					<span>{{ movie.Plot }} </span>
				</p>
				<p><span>Director: </span> {{ movie.Director }}</p>
				<p><span>Genre:</span> {{ movie.Genre }}</p>
				<p><span>Actors:</span> {{ movie.Actors }}</p>
			</div>
		</div>
	</div>
</template>

<script>
import { ref, onBeforeMount } from 'vue';
import { useRoute } from 'vue-router';
import env from '../env.js';

export default {
	setup() {
		const movie = ref({});
		const route = useRoute();

		onBeforeMount(() => {
			window.scroll(0, 0);
			fetch(
				`https://www.omdbapi.com/?apikey=${env.apiKey}&i=${route.params.id}&plot=full`
			)
				.then((res) => res.json())
				.then((data) => {
					movie.value = data;
				});
		});

		return {
			movie,
		};
	},
};
</script>

<style lang="scss" scoped>
.movie-detail {
	width: 90%;
	height: 100vh;

	@media screen and (min-width: 576px) {
		width: 65%;
	}

	h2 {
		margin: 2rem 0;
	}

	.container-flex {
		display: flex;
		flex-direction: column;

		@media screen and (min-width: 576px) {
			flex-direction: row;
		}

		img {
			height: auto;
			width: auto;

			@media screen and (min-width: 576px) {
				max-height: 500px;
			}
		}

		.detail {
			margin-top: 2rem;

			p {
				font-size: 1.2rem;

				span {
					font-size: 1.2rem;
				}
			}

			.plot {
				margin-bottom: 1rem;
			}

			@media screen and (min-width: 576px) {
				width: 50%;
				margin-left: 3rem;
			}
		}
	}
}

span {
	color: #aeb9c9;
}
</style>
