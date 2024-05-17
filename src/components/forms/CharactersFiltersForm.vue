<template>
  <form action="" @submit.prevent="onSubmit">
    <p>
      <label for="sort">Статус персонажа: </label>
      <select name="sort" v-model="status">
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
        v-model="name"
      />
    </p>
    <p>
      <input type="submit" value="Поиск" />
    </p>
  </form>
</template>

<script setup>
import { ref } from "vue";
import defineEmits from "vue";
const name = ref("");
const status = ref("");
const emit = defineEmits(["submit"]);

const onSubmit = () => {
  emit("submit", { status, name });
};
</script>

<style scoped></style>
