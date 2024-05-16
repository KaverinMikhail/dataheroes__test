<template>
  <header>
    <h1>Персонажи из сериала Рик и Морти</h1>
  </header>
  <main>
    <div>
      <select name="sort" @change="sortedPage">
        <option value="?status=">Все персонгажи</option>
        <option value="?status=alive">Alive</option>
        <option value="?status=unknown">Unknown</option>
        <option value="?status=dead">Dead</option>
      </select>
      <input
        type="text"
        placeholder="Имя персонажа"
        v-model="searchCharacterName"
      />
      <button @click="applySort">Поиск песронажа</button>
    </div>

    <div class="main__character__card__all">
      <div
        v-for="item in characterCard"
        :key="item.id"
        class="main__character__card__all__wrapper"
      >
        <div class="main__character__img">
          <img :src="item.image" alt="#" />
        </div>
        <div class="main__character">
          <strong
            ><div class="main__character__name">{{ item.name }}</div></strong
          >
          <div class="main__character__status">{{ item.status }}</div>
          <div class="main__character__species">{{ item.species }}</div>
          <div class="main__character__location">
            Последнее известное местоположение: {{ item.location.name }}
          </div>
          <div class="main__character__episode">
            Впервые был замечен в: {{ item.episode[0] }}
          </div>
        </div>
      </div>
    </div>
    <div class="nextBtn">
      <div v-for="item in pagesCharacterAll" :key="item">
        <button @click="nextPage(item)">{{ item }}</button>
      </div>
    </div>
  </main>
</template>

<script setup>
import { ref } from "vue";
import axios from "axios";
const characterCard = ref([]);
const page = ref(1);
let pagesCharacterAll = 0;
let paramApiName = "";
let paramApiStatus = "?status=";
let params = "";
const searchCharacterName = ref("");

// NEXT PAGE

const nextPage = function (item) {
  page.value = item;
  requestApi();
};

// SORT

const sortedPage = function (value) {
  paramApiStatus = value.target.value;
};

const applySort = function () {
  paramApiName = `&name=${searchCharacterName.value}`;
  page.value = 1;
  params = paramApiStatus + paramApiName;
  requestApi();
};

// API

const requestApi = async function () {
  const response = await axios.get(
    `https://rickandmortyapi.com/api/character${params}`,
    {
      params: {
        page: page.value,
      },
    }
  );
  characterCard.value = response.data.results;
  pagesCharacterAll = response.data.info.pages;

  characterCard.value.forEach((itemUrl) => {
    let idEpisodes = itemUrl.episode[0].split("/");
    episodeApi.push(idEpisodes[idEpisodes.length - 1]);
  });

  firstEpisodesApi();
};

let episodeApi = [];
let episode = [];

// firstEpisodesApi

const firstEpisodesApi = async function () {
  episode = [];
  const response = await axios.get(
    `https://rickandmortyapi.com/api/episode/${episodeApi}`
  );
  episode.push(response.data);

  characterCard.value.forEach((i) => {
    episode.forEach((itemURL) => {
      itemURL.forEach((item) => {
        if (item.url === i.episode[0]) {
          i.episode[0] = item.name;
        }
      });
    });
  });
};

requestApi();
</script>

<style scoped>
header {
  text-align: center;
}
.main__character__card__all {
  display: flex;
  flex-wrap: wrap;
  justify-content: space-around;
  background-color: #272b33;
}
.main__character__card__all__wrapper {
  border-radius: 10px;
  background-color: #3c3e44;
  color: white;
  width: 30%;
  margin-top: 10px;
  margin-bottom: 10px;
  display: flex;
}
.main__character__img img {
  width: 150px;
  height: 100%;
  border-radius: 10px 0 0 10px;
  margin-right: 10px;
}
.main__character {
}
.main__character__name {
  font-size: 32px;
  font-weight: 700;
}
.main__character__status {
}
.main__character__species {
}
.main__character__location {
}
.main__character__episode {
}

.nextBtn {
  display: flex;
  justify-content: center;
}
.nextBtn button {
  margin: 5px;
}
</style>
