<script lang="ts" setup>
import maleIcon from "../../assets/home_page/male.svg";
import femaleIcon from "../../assets/home_page/female.svg";
import { Animal } from "@stores/animalStore.ts";
import { AnimalGender } from "@/enums/animal_gender.ts";
import dogUrl from "@/assets/images/dog_default.jpg";
import catUrl from "@/assets/images/cat_default.jpg";
import { AnimalSpecies } from "@/enums/animal_species.ts";

const { animal } = defineProps<{ animal: Animal }>();

const genero = animal.genero === AnimalGender.MALE ? maleIcon : femaleIcon;
const adoptado = animal.adoptado ? "ADOPTADO" : "NO ADOPTADO";
</script>

<template>
  <a
    class="size-40 md:w-[15rem] h-max max-w-sm bg-white border border-gray-200 rounded-lg shadow"
    href="#"
  >
    <img
      :src="
        animal.imagen ??
        (animal.especie === AnimalSpecies.DOG ? dogUrl : catUrl)
      "
      alt="animal image"
      class="h-52 md:size-64 w-full rounded-t-lg"
    />
    <div class="pl-4 py-3 pr-3 text-[0.8rem]">
      <div class="flex gap-2 w-min items-center mb-2">
        <h5
          class="text-2xl font-bold tracking-tight text-gray-900 dark:text-white"
        >
          {{ animal.nombre }}
        </h5>
        <img :src="genero" alt="" class="size-6" />
      </div>
      <div>
        <p class="text-gray-700 dark:text-gray-400">
          {{ `Aproximadamente ${animal.edad} meses` }}
        </p>
        <span
          :class="{
            'text-[#11720A] font-semibold': animal.adoptado,
            'text-red-950 font-semibold': !animal.adoptado,
          }"
          >{{ adoptado }}</span
        >
      </div>
    </div>
  </a>
</template>
