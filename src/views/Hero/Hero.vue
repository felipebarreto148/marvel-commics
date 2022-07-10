<template>
	<section class="hero">
		<default-card show-header class="hero__card">
			<template v-slot:header>
				<header class="hero__card__header">
					<base-button
						class="hero__card__header__button"
						@clicked="() => $router.push('/home')"
					>
						<span class="mdi mdi-home" />
						Início
					</base-button>
				</header>
			</template>

			<template v-slot:content>
				<section class="hero__card__infos">
					<img :src="renderImage(hero.thumbnail)" alt="" />
					<section>
						<h2 class="hero__card__infos__title">{{ hero.name }}</h2>
						<p class="hero__card__infos__description">
							{{ hasDescription(hero) }}
						</p>
					</section>
				</section>
				<section class="hero__card__comics">
					<h2>Comics</h2>
					<section class="hero__card__comics__infos">
						<default-card
							class="hero__card__comics__infos__card"
							v-for="(item, index) in comics"
							:key="index"
							max-width="280"
							@click="() => goRead(item)"
							:clicked="true"
						>
							<template v-slot:content>
								<section class="hero__card__comics__infos__content">
									<img :src="renderImage(item.thumbnail)" loading="lazy" />
									<h4>{{ item.title }}</h4>
								</section>
							</template>
						</default-card>
					</section>
				</section>
			</template>
		</default-card>
	</section>
</template>

<script>
import { computed, defineComponent, reactive } from "@vue/composition-api";
import { renderImage } from "@/utils/imageHelper";

function hasDescription(hero) {
	return hero.description.trim().length
		? hero.description
		: "description not informed.";
}

function goRead({ urls }) {
	const { url } = urls[0];
	window.open(url, "_blank");
}

export default defineComponent({
	components: {
		DefaultCard: () => import("@/components/Card/Card.vue"),
		BaseButton: () => import("@/components/Buttons/Button/Button.vue"),
	},
	setup(_, { root }) {
		const $store = root.$store;
		const $route = root.$route;
		$store.dispatch("getCharacterById", $route.params.id);
		$store.dispatch("getComics", $route.params.id);

		const state = reactive({});
		const hero = computed(() => $store.getters.hero);
		const comics = computed(() => $store.getters.comics);

		return { state, hero, hasDescription, renderImage, comics, goRead };
	},
});
</script>

<style lang="scss" scoped>
@import "@/scss/_variables.scss";

.hero {
	width: 100%;
	height: 100%;
	display: flex;
	flex-direction: column;
	align-items: center;

	&__card {
		padding: 0;

		&__header {
			width: 100%;
			display: flex;
			justify-content: flex-end;
			align-items: center;
			padding: 10px 20px;

			&__button {
				width: 150px;
				height: 40px;
				display: flex;
				align-items: center;
				justify-content: center;
				column-gap: 5px;
			}
		}

		&__infos {
			display: flex;
			column-gap: 20px;
			row-gap: 20px;
			text-align: left;
			height: 60%;
			padding: 10px 10px 10px 0;

			& img {
				max-height: 700px;
				max-width: 700px;
				border-radius: 0 10px 10px 0;
			}

			& section {
				row-gap: 10px;
				padding: 0 10px;
				font: 500 18px "Poppins", sans-serif;

				.hero__card__infos__description {
					font-size: 16px;
					line-height: 1.5;
				}
			}

			@media (max-width: 768px) {
				flex-direction: column;
				padding: 0;

				& img {
					border-radius: 8px;
					padding: 0 10px;
				}
			}
		}

		&__comics {
			width: 100%;
			height: 50%;
			padding: 10px;

			display: flex;
			flex-direction: column;

			h2 {
				text-align: left;
				font: 500 20px "Poppins", sans-serif;
			}

			img {
				height: 90%;
				width: 100%;
				object-fit: contain;
			}

			&__infos {
				width: 100%;
				display: grid;
				grid-template-columns: repeat(3, 1fr);
        justify-items: center;
				column-gap: 20px;
				row-gap: 20px;
				padding: 10px;

        &__content {
          font: 500 17px "Poppins", sans-serif;
          text-align: left;
          
          h4 {
            padding: 5px;
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
			}
		}
	}
}
</style>
