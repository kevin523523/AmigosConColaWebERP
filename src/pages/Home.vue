<script lang="ts" setup>
import AddButton from "@/components/home_page/AddButton.vue";
import SearchInput from "@/components/home_page/SearchInput.vue";
import Pagination from "@/components/home_page/Pagination.vue";
import FilterButton from "@/components/home_page/FilterButton.vue";
import AnimalCard from "@/components/home_page/AnimalCard.vue";
import catIcon from "@/assets/home_page/cat.svg";
import bellIcon from "@/assets/home_page/bell.svg";
import dogIcon from "@/assets/home_page/dog.svg";
import { onMounted, ref, watch } from "vue";
import { Animal, GetResponse, useAnimals } from "@stores/animalStore.ts";
import { AnimalSpecies } from "@/enums/animal_species.ts";
import { useRouter } from "vue-router";

const router = useRouter();
const animalsToShow = 12;
const squareNumber = ref(3);
const api = useAnimals();

const animalesFiltered = ref<Animal[]>([]);
const search = ref("");
const debounceTimer = ref<number | undefined>(undefined);
const isDogSelect = ref(false);
const isCatSelect = ref(false);
const currentPage = ref(1);
const currentSpecie = ref("");

watch(search, () => {
  clearTimeout(debounceTimer.value);
  debounceTimer.value = window.setTimeout(async () => {
    const response: GetResponse | null = await api.getPaginated(
      currentPage.value,
      animalsToShow,
      currentSpecie.value,
      search.value,
    );
    if (response) {
      animalesFiltered.value = response.data;
    }
  }, 300);
});

onMounted(async () => {
  const response: GetResponse | null = await api.getPaginated(
    1,
    animalsToShow,
    currentSpecie.value,
  );
  if (!response) return;
  animalesFiltered.value = response?.data;
  squareNumber.value = response.totalPages;
});

const selectDogs = async () => {
  if (isDogSelect.value) {
    isDogSelect.value = false;
    currentSpecie.value = "";
    currentPage.value = 1;
    const response = await api.getPaginated(currentPage.value, animalsToShow);
    if (!response) return;
    animalesFiltered.value = response.data;
    squareNumber.value = response.totalPages;
    return;
  }
  currentPage.value = 1;
  currentSpecie.value = AnimalSpecies.DOG;
  await handleChangePage(currentPage.value);

  isDogSelect.value = true;
  isCatSelect.value = false;
};

const selectCats = async () => {
  if (isCatSelect.value) {
    isCatSelect.value = false;
    currentSpecie.value = "";
    currentPage.value = 1;
    const response = await api.getPaginated(currentPage.value, animalsToShow);
    if (!response) return;
    animalesFiltered.value = response.data;
    squareNumber.value = response.totalPages;
    return;
  }
  currentPage.value = 1;
  currentSpecie.value = AnimalSpecies.CAT;
  await handleChangePage(currentPage.value);

  isCatSelect.value = true;
  isDogSelect.value = false;
};

const onRegisterAnimalClicked = () => {
  router.push({
    name: "crear-animal",
  });
};

const handleChangePage = async (page: number) => {
  const response = await api.getPaginated(
    page,
    animalsToShow,
    currentSpecie.value,
  );
  if (!response) return;
  animalesFiltered.value = response.data;
  squareNumber.value = response.totalPages;
  currentPage.value = page;
};

const handleNextPage = async () => {
  if (currentPage.value === squareNumber.value) {
    return;
  }
  currentPage.value += 1;
  const response = await api.getPaginated(
    currentPage.value,
    animalsToShow,
    currentSpecie.value,
  );
  if (!response) return;
  animalesFiltered.value = response.data;
};

const handlePreviousPage = async () => {
  if (currentPage.value === 1) {
    return;
  }
  currentPage.value -= 1;
  const response = await api.getPaginated(
    currentPage.value,
    animalsToShow,
    currentSpecie.value,
  );
  if (!response) return;
  animalesFiltered.value = response.data;
  squareNumber.value = response.totalPages;
};
</script>

<template>
  <div class="flex flex-col justify-center">
    <div
      class="flex justify-start lg:justify-end items-center gap-3 lg:mx-4 mt-5"
    >
      <SearchInput v-model="search" />
      <AddButton @click="onRegisterAnimalClicked">
        <span class="hidden md:flex">Registrar Animal</span>
      </AddButton>
    </div>
    <div class="mt-5 mb-5 flex items-center gap-3">
      <h2 class="text-2xl md:text-4xl font-bold m-0">Categorías</h2>
      <img
        :src="bellIcon"
        alt="notification bell"
        class="mt-1 md:mt-2 size-4 md:size-6"
      />
    </div>
    <div class="flex items-center flex-wrap gap-3 mt-4 mb-7">
      <!-- <FilterButton :icon="filterIcon" text="Filtrar" /> -->
      <span class="font-semibold">Filtrar por especie:</span>
      <FilterButton
        :icon="dogIcon"
        :isSelect="isDogSelect"
        text="Perros"
        @click="selectDogs"
      />
      <FilterButton
        :icon="catIcon"
        :isSelect="isCatSelect"
        text="Gatos"
        @click="selectCats"
      />
    </div>

    <section class="gap-x-5 flex md:gap-x-5 lg:gap-x-11 gap-y-9 flex-wrap mt-2">
      <RouterLink
        v-for="animal in animalesFiltered"
        :key="animal.id"
        :to="`/pet-info/${animal.id}`"
        class="gap-x-5 flex md:gap-x-5 lg:gap-x-11 gap-y-9 flex-wrap mt-2"
      >
        <AnimalCard :animal="animal" />
      </RouterLink>
    </section>
    <div class="flex justify-center py-10">
      <Pagination
        :currentPage="currentPage"
        :pages="squareNumber"
        @nextPage="handleNextPage"
        @pageChange="handleChangePage"
        @previousPage="handlePreviousPage"
      />
    </div>
  </div>
</template>
