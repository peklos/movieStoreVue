<script setup> 
import { motion } from 'motion-v'
import './assets/main.css'
</script>
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
        <h2 class="text-2xl font-bold mb-6">Наш арсенал:</h2>

        <!-- Фильтры -->
        <div class="flex flex-wrap gap-4 mb-6">
          <select
            v-model="selectedFilterGenre"
            class="px-4 py-3 rounded-lg border border-gray-300 bg-white cursor-pointer"
          >
            <option>Все жанры</option>
            <option>Боевик</option>
            <option>Комедия</option>
            <option>Криминал</option>
            <option>Драма</option>
            <option>Фантастика</option>
          </select>
          <select
            v-model="selectedFilterYears"
            class="px-4 py-3 rounded-lg border border-gray-300 bg-white cursor-pointer"
          >
            <option>Все годы</option>
            <option>Новинки 2025</option>
            <option>2020-2024</option>
            <option>2010-2019</option>
            <option>2000-2009</option>
            <option>1990-1999</option>
          </select>
          <input
            v-model="filter"
            type="text"
            placeholder="Поиск..."
            class="px-4 py-3 rounded-lg border border-gray-300 bg-white flex-grow max-w-md"
          />
          <motion.button
            class="flex items-center gap-1 px-4 py-2 border border-gray-300 rounded-lg hover:bg-gray-200 duration-150 cursor-pointer"
            @click="sortOrder == '' || sortOrder == 'asc' ? sortOrder = 'desc' : sortOrder = ''"
            :class="sortOrder == 'desc' ? 'bg-gray-300' : 'bg-white'"
            :whilePress="{scale: 0.9}"
          >
            <span>По убыванию</span>
            <svg
              width="16"
              height="16"
              viewBox="0 0 24 24"
              fill="none"
              xmlns="http://www.w3.org/2000/svg"
            >
              <path
                d="M7 10L12 15L17 10"
                stroke="currentColor"
                stroke-width="2"
                stroke-linecap="round"
                stroke-linejoin="round"
              />
            </svg>
          </motion.button>
          <motion.button
            class="flex items-center gap-1 px-4 py-2 border border-gray-300 rounded-lg hover:bg-gray-200 duration-150"
            @click="sortOrder == '' || sortOrder == 'desc'? sortOrder = 'asc' : sortOrder = ''"
            :class="sortOrder == 'asc' ? 'bg-gray-300' : 'bg-white'"
            :whilePress="{scale: 0.9}"
          >
            <span>По возрастанию</span>
            <svg
              width="16"
              height="16"
              viewBox="0 0 24 24"
              fill="none"
              xmlns="http://www.w3.org/2000/svg"
            >
              <path
                d="M17 14L12 9L7 14"
                stroke="currentColor"
                stroke-width="2"
                stroke-linecap="round"
                stroke-linejoin="round"
              />
            </svg>
          </motion.button>
        </div>

        <!-- Пагинация -->
        <div class="flex justify-center mb-8">
          <div class="flex gap-2">
            <motion.button
              class="px-2 py-2 bg-white border border-gray-300 rounded-lg hover:bg-gray-300 duration-150 disabled:bg-gray-200"
              @click="page -= 1"
              :disabled="page == 1"
              :whilePress="{scale: 0.9}"
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
            </motion.button>
            <button
              class="px-4 py-2 bg-white border border-gray-300 rounded-lg"
            >
              {{ page }}
            </button>
            <motion.button
              class="px-2 py-2 bg-white border border-gray-300 rounded-lg hover:bg-gray-300 duration-150 disabled:bg-gray-200"
              @click="page += 1"
              :disabled="hasNextPage == false"
              :whilePress="{scale: 0.9}"
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
            </motion.button>
          </div>
        </div>

        <!-- Блоки с фильмами (адаптивные) -->
        <div class="grid grid-cols-1 sm:grid-cols-2 lg:grid-cols-4 gap-6">
          <!-- Фильм БЛОК -->
          <motion.div
            class="bg-white rounded-lg shadow-md overflow-hidden hover:shadow-lg transition-shadow border-blue-800 border-solid cursor-pointer"
            v-for="movie in paginatedMovies"
            :key="movie.title"
          >
            <img
              :src="movie.poster"
              :alt="movie.title"
              class="w-full h-96 object-cover"
              loading="lazy"
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
          </motion.div>
        </div>

        <!-- Нижняя пагинация -->
        <div
          class="flex justify-center m-8"
          v-if="paginatedMovies.length > 7 && hasNextPage"
        >
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
            <button
              class="px-4 py-2 bg-white border border-gray-300 rounded-lg"
            >
              {{ page }}
            </button>
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
      filter: "",
      selectedFilterGenre: "Все жанры",
      selectedFilterYears: "Все годы",
      sortOrder: '',
    };
  },

  computed: {
    startIndex() {
      return 8 * (this.page - 1);
    },

    endIndex() {
      return 8 * this.page;
    },

    filteredMovies() {
      const searchTerm = this.filter.toUpperCase();
      const selectedGenre = this.selectedFilterGenre.toUpperCase();
      const selectedYears = this.selectedFilterYears;
      

      const filteredMovii = this.movies.filter((movie) => {
        // Фильтрация по названию
        if (!movie.title.toUpperCase().includes(searchTerm)) {
          return false;
        }

        // Фильтрация по жанру
        if (
          selectedGenre !== "ВСЕ ЖАНРЫ" &&
          !movie.genre[0].toUpperCase().includes(selectedGenre)
        ) {
          return false;
        }

        const year = parseInt(movie.year, 10);

        // Фильтрация по годам
        if (selectedYears === "2020-2024") {
          if (year < 2020 || year > 2024) {
            return false;
          }
        } else if (selectedYears === "2010-2019") {
          if (year < 2010 || year > 2019) {
            return false;
          }
        } else if (selectedYears === "2000-2009") {
          if (year < 2000 || year > 2009) {
            return false;
          }
        } else if (selectedYears === "1990-1999") {
          if (year < 1990 || year > 1999) {
            return false;
          }
        } else if (selectedYears === "Новинки 2025") {
          if (year != 2025) {
            return false;
          }
        }

        return true;
      });

      let result = [...filteredMovii];

      if (this.sortOrder == 'desc') {
        result.sort((a,b) => b.rating - a.rating)
      } else if (this.sortOrder == 'asc') {
        result.sort((a,b) => a.rating - b.rating)
      }

      return result;
    },

    paginatedMovies() {
      return this.filteredMovies.slice(this.startIndex, this.endIndex);
    },

    hasNextPage() {
      return this.filteredMovies.length > this.endIndex;
    },
  },

  created() {},

  methods: {},

  watch: {
    filteredMovies() {
      this.page = 1;
    },

    selectedFilter() {},
  },
};
</script>

<style>
</style>
