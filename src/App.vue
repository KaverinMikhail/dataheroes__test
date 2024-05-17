<template>
  <header>
    <h1>Персонажи из сериала Рик и Морти</h1>
  </header>
  <main>
    <form action="" @submit.prevent="onSubmit">
      <p>
        <label for="sort">Статус персонажа: </label>
        <select name="sort" v-model="paramApiStatus">
          <option value="">Все персонажи</option>
          <option value="alive">Живые</option>
          <option value="unknown">Неизвестно</option>
          <option value="dead">Мертвые</option>
        </select>
      </p>
      <p>
        <label for="name">Имя персонажа: </label>
        <input
          name="name"
          type="text"
          placeholder="Имя персонажа"
          v-model="paramApiName"
        />
      </p>
      <p>
        <input type="submit" value="Поиск" />
      </p>
    </form>

    <div class="main__character__card__all">
      <div
        v-for="item in characters"
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
            Впервые был замечен в: {{ item.debudeEpisode }}
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
const characters = ref([]);
const page = ref(1);
let pagesCharacterAll = 0;
let paramApiName = ref("");
let paramApiStatus = ref("");

// NEXT PAGE

const nextPage = (item) => {
  page.value = item;
  fetchEpisodes();
};

// SORT
const onSubmit = () => {
  page.value = 1;
  fetchEpisodes();
};

// API

const fetchEpisodes = async () => {
  const response = await axios.get(
    `https://rickandmortyapi.com/api/character`,
    {
      params: {
        name: paramApiName.value,
        status: paramApiStatus.value,
        page: page.value,
      },
    }
  );
  characters.value = response.data.results;
  pagesCharacterAll = response.data.info.pages;

  console.log(characters.value[0]);

  firstEpisodesIds();
};

const firstEpisodesIds = async () => {
  let firstEpisodesIdsApi = [];
  let episodes = [];

  characters.value.forEach((itemUrl) => {
    let idEpisodes = itemUrl.episode[0].split("/");
    firstEpisodesIdsApi.push(idEpisodes[idEpisodes.length - 1]);
  });

  const response = await axios.get(
    `https://rickandmortyapi.com/api/episode/${firstEpisodesIdsApi}`
  );

  episodes.push(response.data);

  characters.value.forEach((itemCard) => {
    for (const key in episodes[0]) {
      if (episodes[0][key].url === itemCard.episode[0]) {
        itemCard.debudeEpisode = episodes[0][key].name;
      }
    }
  });
};

fetchEpisodes();
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
