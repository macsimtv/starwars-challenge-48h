<script setup>
import { ref, onMounted } from "vue";
import { useRoute, useRouter } from "vue-router";

import API from "../utils/api";

const route = useRoute();
const router = useRouter();

const params = ref({
  category: route.params.category,
  id: route.params.id,
});

const details = ref({});
const illustration = ref('❔');

onMounted(async () => {
  details.value = await API.getItemDetails(
    params.value.category,
    params.value.id
  );

  setIllustration();
});

const setIllustration = () => {
  switch(params.value.category) {
    case 'films': 
      illustration.value = '🎬';
    break;
    case 'species': 
      illustration.value = '🦕';
    break;
    case 'vehicles': 
      illustration.value = '🏎️';
    break;
    case 'starships': 
      illustration.value = '🚀';
    break;
    case 'people': 
      illustration.value = '🤖';
      if(details.value.gender == 'male') {
        illustration.value = '👨';
      }
      if(details.value.gender == 'female') {
        illustration.value = '👩';
      }
      if(details.value.gender == 'hermaphrodite') {
        illustration.value = '🧔‍♀️';
      }
      
    break;
    case 'planets': 
      illustration.value = '🌚';
    break;
  }
}

const getTableInfo = (data) => {
  let {
    name,
    title,
    films,
    created,
    edited,
    pilots,
    url,
    people,
    homeworld,
    planets,
    characters,
    starships,
    vehicles,
    species,
    residents,
    ...allowed
  } = data;
  return allowed;
};

const onGoBack = () => {
  router.go(-1);
};
</script>

<template>
  <main class="details">
    <div class="details__navigation container">
      <div class="details__menu">
        <a href="#" @click.prevent="onGoBack" class="details__link"
          ><svg
            xmlns="http://www.w3.org/2000/svg"
            class="h-6 w-6"
            fill="none"
            viewBox="0 0 24 24"
            stroke="currentColor"
          >
            <path
              stroke-linecap="round"
              stroke-linejoin="round"
              stroke-width="2"
              d="M11 17l-5-5m0 0l5-5m-5 5h12"
            /></svg
          >Go back</a
        >
      </div>
      <router-link to="/" class="details__home">
        <svg
          xmlns="http://www.w3.org/2000/svg"
          class="h-6 w-6"
          fill="none"
          viewBox="0 0 24 24"
          stroke="currentColor"
        >
          <path
            stroke-linecap="round"
            stroke-linejoin="round"
            stroke-width="2"
            d="M3 12l2-2m0 0l7-7 7 7M5 10v10a1 1 0 001 1h3m10-11l2 2m-2-2v10a1 1 0 01-1 1h-3m-6 0a1 1 0 001-1v-4a1 1 0 011-1h2a1 1 0 011 1v4a1 1 0 001 1m-6 0h6"
          />
        </svg>
      </router-link>
    </div>
    <div class="details__content container">
      <div class="details__head">
        <div class="details__informations">
          <h2 class="details__title heading--h1 heading--stroke">
            {{ details.name || details.title }}
          </h2>
          <div class="details__illu">
            {{ illustration }}
          </div>
        </div>
      </div>
      <div class="details__body">
        <table>
            <tr v-for="(detail, index) in getTableInfo(details)" :key="index">
              <td class="bold">
                {{ index }}
              </td>
              <td>
                {{ detail }}
              </td>
            </tr>
        </table>
      </div>
    </div>
  </main>
</template>
