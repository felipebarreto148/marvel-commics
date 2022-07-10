<template>
  <section class="searcher">
    <input
      class="searcher__field"
      type="text"
      :placeholder="placeholder"
      v-model="state.valueToSearch"
      @input="() => searchCharacter()"
    />
  </section>
</template>

<script>
import { defineComponent, reactive } from "@vue/composition-api";

export default defineComponent({
  props: {
    placeholder: {
      type: String,
      required: true,
      default: () => "Pesquise aqui...",
    },
  },
  setup(_, { root }) {
    const $store = root.$store;
    const state = reactive({
      valueToSearch: null,
    });

    async function searchCharacter() {
      if (!state.valueToSearch.length) {
        $store.commit("setTempList", []);
        $store.dispatch("showNotification", {
          text: "Lista de herois reiniciada!",
          color: "blue",
        });
        return;
      }
      await $store.dispatch("getCharactersByName", state.valueToSearch);
    }

    return {
      state,
      searchCharacter,
    };
  },
});
</script>

<style lang="scss" scoped>
@import "@/scss/_variables.scss";
.searcher {
  width: 100%;
  height: 60px;
  display: flex;
  align-items: center;
  justify-content: flex-end;
  
  @media (max-width: 768px) {
    margin: 0 10px;
    justify-content: center;
  }

  &__field {
    width: 100%;
    max-width: 500px;
    line-height: 30px;
    font: 500 20px "Poppins", sans-serif;
    transition: 200ms;
    border-radius: 5px;
    padding: 5px;
    border: 2px solid $primary;
    outline: none;
    background: white;

    &:focus {
      border: 2px solid $primary;
    }
  }
}
</style>
