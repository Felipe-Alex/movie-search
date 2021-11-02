<template>
	<div class="home">
		<div class="card-container ">
			<div class="main-card">
				<router-link to="/movie/tt3896198">
					<img src="../assets/main-img.jpg" class="main-img" />
					<div class="detail">
						<h3>Guardians of the Galaxy Vol. 2</h3>
						<p>
							The Guardians struggle to keep together as a team while dealing
							with their personal family issues, notably Star-Lord's encounter
							with his father the ambitious celestial being Ego.
						</p>
					</div>
				</router-link>
			</div>
		</div>

		<div class="search-box">
			<input
				@keyup.enter="SearchMovie"
				v-model="search"
				type="text"
				placeholder="Search a movie..."
				ref="boxSearch"
			/>
			<input type="submit" value="Search" @click="SearchMovie" />
		</div>

		<transition name="error-msg">
			<div class="error" v-if="error">
				<p>
					Movie not found!
				</p>
			</div>
		</transition>

		<div class="loader" v-if="loader">
			<img src="../assets/loader.svg" alt="loader" />
		</div>
	</div>

	<div class="movies-list container">
		<div class="movie" v-for="movie in movies" :key="movie.imdbID">
			<router-link :to="'/movie/' + movie.imdbID" class="movie-link">
				<div class="movie-img">
					<img :src="movie.Poster" :alt="movie.Title + ' Poster'" />
					<p class="type">{{ movie.Type }}</p>
				</div>
				<div class="detail">
					<p class="year">{{ movie.Year }}</p>
					<h3>{{ movie.Title }}</h3>
				</div>
			</router-link>
		</div>
	</div>
</template>

<script>
import { ref, onBeforeMount } from 'vue';
import env from '../env.js';
export default {
	name: 'Home',
	components: {},
	setup() {
		const search = ref('');
		const movies = ref([]);
		let loader = ref(false);
		let error = ref(false);

		const moveHeight = () => {
			window.scroll(0, 300);
		};

		const SearchMovie = async () => {
			try {
				if (search.value !== '') {
					loader = true;
					let response = await fetch(
							`https://www.omdbapi.com/?apikey=${env.apiKey}&s=${search.value}`
						),
						json = await response.json();

					if (json.Response === 'True') {
						movies.value = json.Search;
						moveHeight();
					} else {
						error.value = true;
						setTimeout(() => {
							error.value = false;
						}, 2000);
						console.log(json.Error);
					}
					search.value = '';
				}
			} finally {
				window.scroll(0, 300);
				loader = false;
			}
		};

		onBeforeMount(() => {
			fetch(`https://www.omdbapi.com/?apikey=${env.apiKey}&s=marvel`)
				.then((res) => res.json())
				.then((data) => (movies.value = data.Search));
		});

		return {
			search,
			movies,
			error,
			loader,
			SearchMovie,
		};
	},
};
</script>

<style lang="scss" scoped>
.home {
	.card-container {
		height: 80%;
		display: flex;
		justify-content: center;
		align-items: center;
		transition: transform 0.4s ease-in-out;

		.main-card {
			position: relative;
			width: 100%;

			.main-img {
				width: 100%;
				height: 400px;
				object-fit: fill;
				filter: brightness(60%);
			}

			.detail {
				position: absolute;
				left: 0;
				right: 0;
				bottom: 4px;
				padding: 1rem 2rem;
				background-color: rgba(0, 0, 0, 0.7);

				h3 {
					margin-bottom: 1rem;
					@media screen and (min-width: 1200px) {
						font-size: 1.5rem;
					}
				}

				p {
					font-size: 1rem;
					@media screen and (min-width: 1200px) {
						font-size: 1rem;
					}
				}
			}
		}
	}

	.search-box {
		height: 4.5rem;
		display: flex;
		justify-content: center;
		margin-top: 2rem;
		input {
			height: 3rem;
			display: block;
			appearance: none;
			border: none;
			padding: 0.5rem 1rem;
			outline: none;
			font-size: 1rem;
			background: none;

			&[type='text'] {
				margin-right: 0.5rem;
				width: 50%;
				color: #fff;
				background-color: #0f0f1a;
				transition: all 0.2s ease-in-out;

				&::placeholder {
					color: rgb(116, 116, 116);
				}

				&:focus {
					border-bottom: 3px solid rgb(116, 116, 116);
				}

				@media screen and (min-width: 768px) {
					width: 40%;
				}

				@media screen and (min-width: 900px) {
					width: 30%;
				}
			}

			&[type='submit'] {
				background-color: #649ee4;
				cursor: pointer;
			}
		}
	}

	.error {
		display: flex;
		justify-content: center;
		margin: 1rem 0;

		p {
			padding: 0.2rem 1rem;
			text-align: center;
			color: #953b39;
			font-size: 1.5rem;
			background-color: #f2dede;
			font-weight: bold;
		}
	}

	.error-msg-enter-active {
		animation: move 1s;
	}

	.error-msg-leave-active {
		animation: move 1s reverse;
	}

	@keyframes move {
		0% {
			transform: translateX(0px);
		}
		25% {
			transform: translateX(10px);
		}
		35% {
			transform: translateX(-10px);
		}
		45% {
			transform: translateX(10px);
		}
		55% {
			transform: translateX(-10px);
		}
		65% {
			transform: translateX(10px);
		}
		75% {
			transform: translateX(-10px);
		}
		85% {
			transform: translateX(10px);
		}
		100% {
			transform: translateX(-10px);
		}
	}

	.loader {
		display: flex;
		justify-content: center;
		margin: -2rem 0 2rem;
	}
}

.movies-list {
	display: grid;
	grid-template-columns: repeat(1, 300px);
	justify-content: center;
	grid-gap: 1rem;
	overflow: hidden;

	@media screen and(min-width: 768px) {
		grid-template-columns: repeat(2, 300px);
	}

	@media screen and (min-width: 992px) {
		grid-template-columns: repeat(3, 300px);
	}

	.movie {
		width: auto;
		padding: 1rem 0.5rem;
		transition: transform 0.4s ease-in-out;
		height: 500px;

		@media screen and (min-width: 992px) {
			&:last-child {
				grid-column: 2/3;
			}
		}

		&:last-child {
			&:hover {
				overflow: hidden;
			}
		}

		&:hover {
			transform: scale(1.06);
		}

		.movie-link {
			display: flex;
			flex-direction: column;
			align-items: center;

			.movie-img {
				position: relative;
				text-align: center;

				img {
					height: 350px;
					width: 230px;
					border: 4px solid #fff;
					transition: border 0.2s ease-in-out;
				}

				.type {
					position: absolute;
					padding: 0.5rem 1rem;
					background-color: #0f0f1a;
					left: 0;
					top: 55%;
					color: #fff;
					text-transform: capitalize;
					font-weight: bold;
					font-size: 0.9rem;
					border: 3px solid #fff;
					transition: border 0.2s ease-in-out;
				}
			}

			.detail {
				display: flex;
				flex-direction: column;
				align-items: center;
				justify-content: center;

				h3 {
					text-align: center;
				}
			}
		}
	}
}
</style>
