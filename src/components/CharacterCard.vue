<template>
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
</template>

<script setup>
import defineProps from "vue";
defineProps({
  characters: {
    type: Array,
    required: true,
  },
});
</script>

<style scoped>
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
</style>
