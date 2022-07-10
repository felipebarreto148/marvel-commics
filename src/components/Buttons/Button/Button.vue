<template>
	<button @click="$emit('clicked')">
		<slot v-if="!loading" />
		<span v-else class="mdi mdi-loading mdi-spin" />
	</button>
</template>

<script lang="ts">
import { defineComponent } from "@vue/composition-api";

export default defineComponent({
	name: "Button",
	props: {
		loading: {
			type: Boolean,
			default: false,
		},
	}
});
</script>

<style lang="scss" scoped>
@import "@/scss/_variables.scss";

button {
	height: 50px;
	width: 100%;
	max-width: 200px;
	border: none;
	transition: ease-in 200ms;
	cursor: pointer;

	background: $primary;
	color: $secondary;
	border-radius: 5px;

	font: 500 18px "Poppins", sans-serif;

	& span.mdi {
		font-size: 30px;
	}

	&:hover {
		transform: scale3d(1.1, 1.1, 1.1);
	}
}
</style>
