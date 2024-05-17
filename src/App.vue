<template>
  <header>
    <h1>Персонажи из сериала Рик и Морти</h1>
  </header>
  <main>
    <CharactersFiltersForm @submit="form"></CharactersFiltersForm>
    <div v-if="!isLoading">
      <CharacterCard :characters="characters"></CharacterCard>
      <PaginationFooter
        @page="pagination"
        :pagesCharacterAll="pagesCharacterAll"
      ></PaginationFooter>
    </div>
    <div v-else>Идет загрузка....</div>
  </main>
</template>

<script setup>
import CharactersFiltersForm from "./components/forms/CharactersFiltersForm.vue";
import CharacterCard from "./components/CharacterCard.vue";
import PaginationFooter from "./components/PaginationFooter.vue";

import { ref } from "vue";
import axios from "axios";

const name = ref("");
const status = ref("");
const characters = ref([]);
const page = ref(1);
const pagesCharacterAll = ref(0);
const isLoading = ref(false);

const form = (emit) => {
  name.value = emit.name.value;
  status.value = emit.status.value;
  page.value = 1;
  fetchEpisodes();
};

const pagination = (emit) => {
  page.value = emit;
  fetchEpisodes();
};

const fetchEpisodes = async () => {
  isLoading.value = true;
  const response = await axios.get(
    `https://rickandmortyapi.com/api/character`,
    {
      params: {
        name: name.value,
        status: status.value,
        page: page.value,
      },
    }
  );
  characters.value = response.data.results;
  pagesCharacterAll.value = response.data.info.pages;
  isLoading.value = false;
  firstEpisodesIds();
};

const firstEpisodesIds = async () => {
  let firstEpisodesIdsApi = [];
  isLoading.value = true;
  characters.value.forEach((itemUrl) => {
    let idEpisodes = itemUrl.episode[0].split("/");
    firstEpisodesIdsApi.push(idEpisodes[idEpisodes.length - 1]);
  });
  const response = await axios.get(
    `https://rickandmortyapi.com/api/episode/${firstEpisodesIdsApi}`
  );
  const resultEpisodes = Object.groupBy(response.data, ({ url }) => url);
  characters.value.forEach((itemCard) => {
    if (resultEpisodes[itemCard.episode[0]][0].url === itemCard.episode[0]) {
      itemCard.debudeEpisode = resultEpisodes[itemCard.episode[0]][0].name;
    }
  });
  isLoading.value = false;
};

fetchEpisodes();
</script>

<style scoped>
header {
  text-align: center;
}
</style>
