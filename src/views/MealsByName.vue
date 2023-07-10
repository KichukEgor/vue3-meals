<template>
  <section>
    <div class="p-8 pb-0">
      <h1 class="text-4xl font-bold mb-4 text-orange-500">Search Meals by Name</h1>
    </div>
    <div class="px-8 pb-3">
      <input
        type="text"
        v-model="keyword"
        class="rounded border-2 bg-white border-gray-200 focus:ring-orange-500 focus:border-orange-500 w-full"
        placeholder="Search for Meals"
        @input="searchMeals"
      />
    </div>
  </section>

  <Loading v-if="loading"/>
  <Meals v-else :meals="meals"/>
</template>

<script setup>
import {computed, onMounted, ref} from "vue";
import {useRoute} from "vue-router";
import {useStore} from "vuex";
import Meals from "../components/Meals.vue";
import Loading from "../components/Loading.vue";

const route = useRoute();
const store = useStore();
const keyword = ref("");
const meals = computed(() => store.state.searchedMeals);
const loading = ref(false);

function searchMeals() {
  if (keyword.value) {
    loading.value = true;
    store.dispatch("searchMeals", keyword.value)
      .finally(() => {
        loading.value = false;
      });
  } else {
    store.commit("setSearchedMeals", []);
  }
}

onMounted(() => {
  keyword.value = route.params.name;
  if (keyword.value) {
    searchMeals();
  }
});
</script>
