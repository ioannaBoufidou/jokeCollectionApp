<template>
  <div id="app" class="flex h-screen">
    <aside
      id="separator-sidebar"
      class="w-80 h-full bg-gray-50 overflow-y-auto"
      aria-label="Sidebar"
    >
      <div class="h-full px-3 py-4 overflow-y-auto">
        <header>
          <img
            src="./assets/logo.jpg"
            alt="User profile"
            class="w-12 h-12 rounded-full object-cover"
          />
          <h1 class="text-xl font-bold">Joke Collection</h1>
        </header>
        <ul class="space-y-2 font-medium">
          <li>
            <button
              type="button"
              class="flex items-center w-full p-2 text-base text-gray-900 transition duration-75 rounded-lg group hover:bg-gray-100 dark:text-white dark:hover:bg-gray-700"
              aria-controls="dropdown"
              data-collapse-toggle="dropdown"
              @click="changeView('all')"
            >
              <!-- TODO: Fix the dropdown -->
              <span class="flex-1 ms-3 text-left whitespace-nowrap">All Jokes</span>
              <svg
                class="w-3 h-3"
                xmlns="http://www.w3.org/2000/svg"
                fill="none"
                viewBox="0 0 10 6"
                aria-hidden="true"
              >
                <path
                  stroke="currentColor"
                  stroke-linecap="round"
                  stroke-linejoin="round"
                  stroke-width="2"
                  d="M1 1l4 4 4-4"
                />
              </svg>
            </button>
            <ul id="dropdown" class="hidden py-2 space-y-2">
              <li>
                <a
                  href="#"
                  class="flex items-center w-full p-2 text-gray-900 transition duration-75 rounded-lg pl-11 group hover:bg-gray-100 dark:text-white dark:hover:bg-gray-700"
                >
                  Random Jokes
                </a>
              </li>
              <li>
                <a
                  href="#"
                  class="flex items-center w-full p-2 text-gray-900 transition duration-75 rounded-lg pl-11 group hover:bg-gray-100 dark:text-white dark:hover:bg-gray-700"
                >
                  Programming
                </a>
              </li>
            </ul>
          </li>
          <li>
            <a
              href="#"
              class="flex items-center p-2 text-gray-900 rounded-lg dark:text-white hover:bg-gray-100 dark:hover:bg-gray-700 group"
              @click="changeView('favorites')"
            >
              <span class="ms-3">Favorites</span>
            </a>
          </li>
        </ul>
        <div class="fixed bottom-0 flex items-center gap-4 p-4 border-t border-gray-200 mt-8">
          <img
            src="./assets/profilePhoto.jpg"
            alt="User profile"
            class="w-12 h-12 rounded-full object-cover"
          />
          <div>
            <p class="text-gray-800 font-semibold">John Doe</p>
            <p class="text-gray-500 text-sm">Greece</p>
          </div>
        </div>
      </div>
    </aside>
    <main class="flex-1 bg-gray-100 p-6 overflow-y-auto">
      <header v-if="activeView !== 'favorites'" class="flex justify-center mb-8">
        <div class="flex bg-gray-200 rounded-lg overflow-hidden">
          <button
            class="px-4 py-2 text-gray-800 hover:bg-gray-300 transition"
            :class="{ 'bg-gray-300': activeCategory === 'random' }"
            @click="changeCategory('random')"
          >
            Random jokes
          </button>
          <button
            class="px-4 py-2 text-gray-800 hover:bg-gray-300 transition"
            :class="{ 'bg-gray-300': activeCategory === 'programming' }"
            @click="changeCategory('programming')"
          >
            Programming jokes
          </button>
        </div>
      </header>
      <div v-if="spinner" class="flex flex-col items-center">
        <div class="animate-spin h-8 w-8 border-4 border-t-transparent rounded-full"></div>
        <span>Fetching jokes... Your daily dose of laughter is on its way!</span>
      </div>
      <div
        v-else-if="activeView === 'favorites' && displayedJokes.length === 0"
        class="flex flex-col items-center justify-center h-full"
      >
        <span
          >Looks like your Favorites list could use a boost. Go back to All Jokes to add some jokes
          to your Favorites list.</span
        >
      </div>
      <div v-else>
        <div class="grid grid-cols-3 gap-8 justify-center pb-8">
          <JokeInfo
            v-for="joke in displayedJokes"
            :key="joke.id"
            :type="joke.type"
            :setup="joke.setup"
            :punchline="joke.punchline"
            :id="joke.id"
            :isFavorite="favoriteJokes.includes(joke.id)"
            @add-favorite="addToFavorite"
          />
        </div>
        <!-- TODO: Create maybe a magenta color for show more button, also for the arrow -->
        <div v-if="activeView !== 'favorites'" class="flex justify-center mt-6">
          <button
            class="text-gray-600 hover:text-gray-800 underline text-lg"
            @click="showMoreJokes"
          >
            Show more
          </button>
        </div>
      </div>
    </main>
    <button
      class="fixed bottom-4 right-4 p-3 rounded-full bg-gray-600 text-white hover:bg-gray-700"
      @click="scrollToTop"
    >
      ^
    </button>
  </div>
</template>
<script>
import JokeInfo from './components/JokeInfo.vue'

export default {
  name: 'App',
  components: {
    JokeInfo,
  },
  data() {
    return {
      activeCategory: 'random',
      jokes: [],
      spinner: false,
      favoriteJokes: [],
      activeView: 'all',
    }
  },

  methods: {
    async fetchJokes() {
      try {
        this.spinner = true
        const url =
          this.activeCategory === 'random'
            ? 'https://official-joke-api.appspot.com/jokes/ten'
            : 'https://official-joke-api.appspot.com/jokes/programming/ten'

        const response = await fetch(url)
        if (!response.ok) throw new Error('Failed to fetch jokes')
        const newJokes = await response.json()
        this.jokes = [...this.jokes, ...newJokes]
      } catch (error) {
        this.jokes = []
        alert('Failed to load jokes. Please try to refresh your page.')
        console.error('Error fetching jokes:', error)
      } finally {
        this.spinner = false
      }
    },
    async showMoreJokes() {
      await this.fetchJokes()
    },
    scrollToTop() {
      window.scrollTo(0, 0) // TODO: Fix the navigation to the top of the page
    },
    changeCategory(category) {
      if (this.activeCategory === category) return
      this.activeCategory = category
      this.jokes = []
      this.fetchJokes()
    },
    changeView(view) {
      this.activeView = view
    },
    addToFavorite(jokeId) {
      this.favoriteJokes.push(jokeId)
    },
  },
  created() {
    this.fetchJokes()
  },
  computed: {
    displayedJokes() {
      return this.activeView === 'all'
        ? this.jokes
        : this.jokes.filter((joke) => this.favoriteJokes.includes(joke.id))
    },
  },
}
</script>

<style>
@tailwind base;
@tailwind components;
@tailwind utilities;

#app {
  height: 100%;
  margin: 0;
  padding: 0;
  width: 100%;
}

header {
  border-radius: 12px;
  box-shadow: 0 4px 8px rgba(0, 0, 0, 0.1);
  margin: 2rem auto;
  padding: 1rem 2rem;
  max-width: 1000px;
  text-align: center;
  display: flex;
  align-items: center;
  gap: 0.5rem;
}
</style>
