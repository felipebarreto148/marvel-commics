<template>
	<section class="home">
		<section class="home__characters">
			<transition name="fade" appear mode="out-in" v-for="(hero, index) in characters" :key="index">
				<default-card
					max-width="300"
					@click="$router.push(`/hero/${hero.id}`)"
					:clicked="true"
					class="home__characters__card"
				>
					<template v-slot:content>
						<section class="home__characters__content">
							<img :src="renderImage(hero.thumbnail)" loading="lazy" />
							<h3 class="hero__name">{{ hero.name }}</h3>
						</section>
					</template>
				</default-card>
			</transition>
		</section>

		<base-button
			v-if="characters.length"
			@clicked="() => init()"
			:loading="state.isLoading"
		>
			Carregar mais
		</base-button>
	</section>
</template>

<script>
import { defineComponent, reactive, computed } from "@vue/composition-api";
import { renderImage } from "@/utils/imageHelper";

export default defineComponent({
	components: {
		DefaultCard: () => import("@/components/Card/Card.vue"),
		Searcher: () => import("@/components/Inputs/Searcher/Searcher.vue"),
		BaseButton: () => import("@/components/Buttons/Button/Button.vue"),
	},
	setup(_, { root }) {
		const $store = root.$store;
		const state = reactive({
			characters: $store.getters.characters,
			isLoading: false,
		});

		const characters = computed(() => $store.getters.characters);

		async function init() {
			state.isLoading = true;
			await $store
				.dispatch("getCharacters")
				.then(() => (state.isLoading = false));
		}

		init();

		return {
			init,
			state,
			characters,
			renderImage,
		};
	},
});
</script>

<style lang="scss" scoped>
@import "@/scss/_variables.scss";

.home {
	width: 100%;
	display: flex;
	flex-direction: column;
	padding: 10px;
	row-gap: 20px;
	align-items: center;

	&__searcher {
		height: 100%;
		max-height: 100px;
		width: 100%;
		display: flex;
		align-items: center;
	}

	&__characters {
		height: 100%;
		width: 100%;

		display: grid;
		grid-template-columns: repeat(4, 1fr);
		justify-items: center;
		row-gap: 30px;
		column-gap: 30px;

		@media (max-width: 1350px) {
			grid-template-columns: 1fr 1fr;

			@media (max-width: 1000px) {
				grid-template-columns: 1fr;
			}
		}

		&__card:hover {
			transform: scale3D(0.9, 0.9, 0.9);
			&::after {
				content: "";
				display: block;
				background: rgba($primary, 0.5);
				background-image: url("../../assets/icon-view.svg");
				background-position: center;
				background-repeat: no-repeat;
				width: 100%;
				height: 100%;
				position: absolute;
			}
		}

		&__content {
			max-width: 100%;
			height: 100%;
			display: flex;
			flex-direction: column;
			column-gap: 10px;

			img {
				width: 100%;
				max-height: 300px;
				min-width: 100%;
				min-height: 300px;
			}
			.hero__name {
				font: 700 18px "Poppins", sans-serif;
				text-align: left;
				padding: 20px;
			}
		}
	}
}
</style>
