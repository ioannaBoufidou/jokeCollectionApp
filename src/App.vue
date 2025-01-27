<template>
  <div id="app" class="flex h-screen">
    <aside class="w-80 h-full bg-gray-50 overflow-y-auto" aria-label="Sidebar">
      <div class="h-full px-4 py-6">
        <header class="mb-6">
          <img
            src="./assets/logo.jpg"
            alt="Logo"
            class="w-14 h-14 rounded-full object-cover mb-2"
          />
          <h1 class="text-2xl font-extrabold text-gray-800">Joke Collection</h1>
        </header>
        <ul class="font-medium">
          <li>
            <button
              type="button"
              class="flex items-center w-full px-3 py-2 text-lg text-gray-800 font-semibold transition duration-75 rounded-lg group hover:bg-gray-100 dark:text-white dark:hover:bg-gray-700"
              @click="toggleDropdown"
              :aria-expanded="dropdownOpen"
            >
              <span class="flex-1 text-left">All Jokes</span>
              <svg
                class="w-4 h-4 text-gray-600"
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
            <ul
              v-show="dropdownOpen"
              class="py-2 pl-6 space-y-3 border-l border-gray-300"
              role="menu"
            >
              <li>
                <a
                  class="block w-full px-3 py-2 text-base text-gray-700 rounded-lg group hover:bg-gray-200"
                  @click="changeCategory('random')"
                >
                  Random Jokes
                </a>
              </li>
              <li>
                <a
                  class="block w-full px-3 py-2 text-base text-gray-700 rounded-lg group hover:bg-gray-200"
                  @click="changeCategory('programming')"
                >
                  Programming
                </a>
              </li>
            </ul>
          </li>
          <li>
            <a
              class="flex items-center px-3 py-2 text-lg text-gray-800 font-semibold rounded-lg hover:bg-gray-100 transition duration-75 group"
              @click="changeView('favorites')"
            >
              <span class="text-left px-3">Favorites</span>
            </a>
          </li>
        </ul>
        <div class="fixed bottom-0 w-full px-4 py-4 bg-white border-t border-gray-200">
          <div class="flex items-center gap-4">
            <img
              src="./assets/profilePhoto.jpg"
              alt="User profile"
              class="w-14 h-14 rounded-full object-cover"
            />
            <div>
              <p class="text-lg text-gray-900 font-semibold">John Doe</p>
              <p class="text-sm text-gray-500">Greece</p>
            </div>
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
        <div>
          <h2 class="text-2xl font-bold tracking-tight text-gray-900 py-4 px-4">
            {{ HeaderJokeCards }}
          </h2>
          <div class="grid grid-cols-3 gap-8 justify-center pb-8">
            <JokeInfo
              v-for="joke in displayedJokes"
              :key="joke.id"
              :type="joke.type"
              :setup="joke.setup"
              :punchline="joke.punchline"
              :id="joke.id"
              :isFavorite="favoriteJokes.includes(joke.id)"
              @toggle-favorite="toggleFavorite"
            />
          </div>
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
      jokes: { random: [], programming: [] },
      spinner: false,
      favoriteJokes: [],
      activeView: 'all',
      dropdownOpen: false,
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
        if (!this.jokes[this.activeCategory]) {
          //initilize category
          this.jokes[this.activeCategory] = []
        }

        this.jokes[this.activeCategory] = [
          ...this.jokes[this.activeCategory],
          ...newJokes.map((joke) => ({
            ...joke,
            isFavorite: this.favoriteJokes.some((fav) => fav.id === joke.id),
          })),
        ]
      } catch (error) {
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
      this.changeView('all')
      if (this.activeCategory === category) return
      this.activeCategory = category
      this.jokes = []
      this.fetchJokes()
    },
    changeView(view) {
      this.activeView = view
    },
    toggleFavorite(jokeId) {
      const jokeIndex = this.favoriteJokes.findIndex((joke) => joke.id === jokeId)
      if (jokeIndex !== -1) {
        this.favoriteJokes.splice(jokeIndex, 1)
      } else {
        const jokeToAdd = this.jokes[this.activeCategory].find((joke) => joke.id === jokeId)
        if (jokeToAdd) {
          jokeToAdd.isFavorite = true
          this.favoriteJokes.push(jokeToAdd)
        }
      }
    },
    toggleDropdown() {
      this.dropdownOpen = !this.dropdownOpen
    },
  },
  created() {
    this.fetchJokes()
  },
  computed: {
    displayedJokes() {
      return this.activeView === 'favorites'
        ? this.favoriteJokes
        : this.jokes[this.activeCategory] || []
    },
    HeaderJokeCards() {
      return this.activeView === 'favorites'
        ? 'Your Favorite Jokes'
        : this.activeCategory === 'random'
          ? 'Random Jokes'
          : 'Programming Jokes'
    },
  },
}
</script>

<style>
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
