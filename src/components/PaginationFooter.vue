<template>
  <div class="footerBtnNext">
    <div v-for="item in pagesCharacterAll" :key="item">
      <button @click="$emit('page', item)">{{ item }}</button>
    </div>
  </div>
</template>

<script setup>
import defineProps from "vue";

defineProps({
  pagesCharacterAll: {
    type: Array,
    default: () => [],
  },
});
</script>

<style scoped>
.footerBtnNext {
  display: flex;
  justify-content: center;
}
.footerBtnNext button {
  margin: 5px;
}
</style>
