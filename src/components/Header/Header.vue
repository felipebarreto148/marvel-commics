<template>
	<header class="header">
		<img
			src="../../assets/marvel-logo.png"
			alt="Logo da marvel, imagem retangular com o fundo vermelho e a palavra marvel escrita em branco."
			class="header__logo"
			@click="
				$route.path != '/' && $route.path != '/home' ? $router.push('/') : {}
			"
		/>
		<searcher placeholder="Pesquise pelo nome de um personagem." v-if="$route.name == 'Home'"/>
	</header>
</template>

<script lang="ts">
import { defineComponent } from "@vue/composition-api";

export default defineComponent({
	components: {
		Searcher: () => import("@/components/Inputs/Searcher/Searcher.vue"),
	},
	// setup() {},
});
</script>

<style lang="scss" scoped>
.header {
	width: 100vw;
	height: 100%;
	max-height: 100px;
	background: black;
	box-shadow: 0 0 15px rgba($color: #000000, $alpha: 0.8);

	display: flex;
	align-items: center;
	justify-content: space-between;
	padding: 0 5%;
	transition: 500ms;

  @media (max-width: 768px) {
	  max-height: 180px;
    flex-direction: column;
	  padding: 2%;
  }

	&:hover {
		box-shadow: 0 0 25px rgba($color: #000000, $alpha: 0.8);
	}

	&__logo {
		max-height: 70px;
		cursor: pointer;
	}
}
</style>
