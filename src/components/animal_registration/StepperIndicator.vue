<script lang="ts" setup>
const step = defineModel();
</script>

<template>
  <ol class="flex items-center mt-5">
    <li class="relative w-full mb-10 lg:mb-5">
      <div class="flex items-center lg:w-96">
        <div
          class="z-10 flex items-center justify-center w-6 h-6 bg-primary rounded-full ring-0 ring-white sm:ring-8 shrink-0"
        >
          <svg
            aria-hidden="true"
            class="w-2.5 h-2.5 text-blue-100 dark:text-blue-300"
            fill="none"
            viewBox="0 0 16 12"
            xmlns="http://www.w3.org/2000/svg"
          >
            <path
              d="M1 5.917 5.724 10.5 15 1.5"
              stroke="currentColor"
              stroke-linecap="round"
              stroke-linejoin="round"
              stroke-width="2"
            />
          </svg>
        </div>
        <div class="flex-grow bg-gray-200 h-0.5 dark:bg-gray-700"></div>
      </div>
      <div class="mt-3">
        <h3 class="text-sm text-gray-900 dark:text-white">Información</h3>
      </div>
    </li>
    <li class="relative w-full mb-10 lg:mb-5">
      <div class="flex items-center lg:w-96">
        <div
          :class="{
            'z-10 flex items-center justify-center w-6 h-6 bg-primary rounded-full ring-0 ring-white sm:ring-8 shrink-0':
              step > 0,
            'z-10 flex items-center justify-center w-6 h-6 bg-gray-300 rounded-full ring-0 ring-white sm:ring-8 shrink-0':
              step < 1,
          }"
        >
          <svg
            aria-hidden="true"
            class="w-2.5 h-2.5 text-blue-100 dark:text-blue-300"
            fill="none"
            viewBox="0 0 16 12"
            xmlns="http://www.w3.org/2000/svg"
          >
            <path
              d="M1 5.917 5.724 10.5 15 1.5"
              stroke="currentColor"
              stroke-linecap="round"
              stroke-linejoin="round"
              stroke-width="2"
            />
          </svg>
        </div>
        <div class="flex-grow bg-gray-200 h-0.5 dark:bg-gray-700"></div>
      </div>
      <div class="mt-3">
        <h3 class="text-sm text-gray-900 dark:text-white">Subir foto</h3>
      </div>
    </li>
    <li class="relative w-full mb-10 lg:mb-5">
      <div class="flex items-center lg:w-72">
        <div
          :class="{
            'z-10 flex items-center justify-center w-6 h-6 bg-primary rounded-full ring-0 ring-white sm:ring-8 shrink-0':
              step > 1,
            'z-10 flex items-center justify-center w-6 h-6 bg-gray-300 rounded-full ring-0 ring-white sm:ring-8 shrink-0':
              step < 2,
          }"
        >
          <svg
            aria-hidden="true"
            class="w-2.5 h-2.5 text-blue-100 dark:text-blue-300"
            fill="none"
            viewBox="0 0 16 12"
            xmlns="http://www.w3.org/2000/svg"
          >
            <path
              d="M1 5.917 5.724 10.5 15 1.5"
              stroke="currentColor"
              stroke-linecap="round"
              stroke-linejoin="round"
              stroke-width="2"
            />
          </svg>
        </div>
      </div>
      <div class="mt-3">
        <h3 class="text-sm text-gray-900 dark:text-white">Previsualización</h3>
      </div>
    </li>
  </ol>
</template>

<style scoped></style>
