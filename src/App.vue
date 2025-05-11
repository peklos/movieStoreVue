<template>
  <div class="min-h-screen bg-gray-100">
    <!-- Заголовок -->
    <header class="bg-blue-600 text-white py-6">
      <div class="container mx-auto px-4">
        <h1 class="text-3xl font-bold">Кинотека</h1>
        <p class="mt-2">Лучшие фильмы всех времён</p>
      </div>
    </header>

    <!-- Основной контент -->
    <main class="container mx-auto px-4 py-8">
      <!-- Блок "Кино у нас есть" -->
      <section class="mb-12">
        <h2 class="text-2xl font-bold mb-6">Кино у нас есть</h2>

        <!-- Фильтры -->
        <div class="flex flex-wrap gap-4 mb-6">
          <select class="px-4 py-2 rounded-lg border border-gray-300 bg-white">
            <option>Все жанры</option>
            <option>Боевик</option>
            <option>Комедия</option>
          </select>
          <select class="px-4 py-2 rounded-lg border border-gray-300 bg-white">
            <option>Все годы</option>
            <option>2020-2024</option>
            <option>2010-2019</option>
          </select>
          <input
            type="text"
            placeholder="Поиск..."
            class="px-4 py-2 rounded-lg border border-gray-300 bg-white flex-grow max-w-md"
          />
        </div>

        <!-- Пагинация -->
        <div class="flex justify-center mb-8">
          <div class="flex gap-2">
            <button
              class="px-2 py-2 bg-white border border-gray-300 rounded-lg hover:bg-gray-300 duration-150 disabled:bg-gray-200"
              @click="page -= 1"
              :disabled="page == 1"
            >
              <svg
                width="24"
                height="24"
                viewBox="0 0 24 24"
                fill="none"
                xmlns="http://www.w3.org/2000/svg"
              >
                <path
                  d="M15 18L9 12L15 6"
                  stroke="currentColor"
                  stroke-width="2"
                  stroke-linecap="round"
                  stroke-linejoin="round"
                />
              </svg>
            </button>
            <button class="px-4 py-2 bg-white border border-gray-300 rounded-lg">
              {{page}}
            </button>
            <!-- <button
              class="px-4 py-2 bg-white border border-gray-300 rounded-lg"
            >
              2
            </button>
            <button
              class="px-4 py-2 bg-white border border-gray-300 rounded-lg"
            >
              3
            </button> -->
            <button
              class="px-2 py-2 bg-white border border-gray-300 rounded-lg hover:bg-gray-300 duration-150 disabled:bg-gray-200"
              @click="page += 1"
              :disabled="hasNextPage == false"
            >
              <svg
                width="24"
                height="24"
                viewBox="0 0 24 24"
                fill="none"
                xmlns="http://www.w3.org/2000/svg"
              >
                <path
                  d="M9 18L15 12L9 6"
                  stroke="currentColor"
                  stroke-width="2"
                  stroke-linecap="round"
                  stroke-linejoin="round"
                />
              </svg>
            </button>
          </div>
        </div>

        <!-- Блоки с фильмами (адаптивные) -->
        <div class="grid grid-cols-1 sm:grid-cols-2 lg:grid-cols-4 gap-6">
          <!-- Фильм 1 -->
          <div
            class="bg-white rounded-lg shadow-md overflow-hidden hover:shadow-lg transition-shadow border-blue-800 border-solid cursor-pointer"
            v-for="movie in paginatedMovies"
            :key="movie.title"
          >
            <img
              :src="movie.poster"
              alt="Movie 1"
              class="w-full h-96 object-cover"
            />
            <div class="p-5">
              <h3 class="font-bold text-lg mb-1">{{ movie.title }}</h3>
              <p class="text-gray-600 text-sm">
                {{ movie.year }} · {{ movie.genre[0].toUpperCase() }}
              </p>
              <p class="text-gray-600 text-sm py-4">{{ movie.description }}</p>
              <div class="mt-2 flex justify-between items-center">
                <span class="text-yellow-500">★ {{ movie.rating }}</span>
                <button
                  class="px-3 py-1 bg-blue-600 text-white rounded text-sm"
                >
                  Смотреть
                </button>
              </div>
            </div>
          </div>
        </div>
      </section>
    </main>
  </div>
</template>

<script>
import moviesData from "../vendor/data.json";

export default {
  data() {
    return {
      movies: moviesData.movies,
      page: 1,
    };
  },

  computed: {
    startIndex() {
      return 8 * (this.page - 1);
    },

    endIndex() {
      return 8 * this.page;
    },

    paginatedMovies() {
      return this.movies.slice(this.startIndex, this.endIndex);
    },

    hasNextPage() {
      return this.movies.length > this.endIndex;
    },
  },

  created() {},

  methods: {},

  watch: {},
};
</script>

<style>
</style>
