<template>
  <Loading v-if="loading" />
  <div v-else>
    <div class="p-8 pb-0 text-orange-500">
      <h1 class="text-4xl font-bold mb-4">Daily Random Meals</h1>
    </div>
    <Meals :meals="meals" />
  </div>
</template>

<script setup>
import { onMounted, ref } from "vue";
import Meals from "../components/Meals.vue";
import Loading from "../components/Loading.vue";
import axiosClient from "../axiosClient.js";

const meals = ref([]);
const loading = ref(true);

onMounted(() => {
  fetchRandomMeals(10);
});

async function fetchRandomMeals(count) {
  try {
    loading.value = true;

    const requests = Array.from({ length: count }, () => axiosClient.get("random.php"));
    const responses = await Promise.all(requests);
    meals.value = responses.map(({ data }) => data.meals[0]);

    loading.value = false;
  } catch (error) {
    console.error("Error fetching random meals:", error);
    loading.value = false;
  }
}
</script>
