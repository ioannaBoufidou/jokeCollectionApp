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
        <h2 class="text-sm font-semibold text-gray-500 tracking-wider mb-4">COLLECTIONS</h2>
        <ul class="font-medium">
          <li>
            <button
              type="button"
              class="flex items-center justify-between w-full px-2 py-4 text-lg text-magenta-700 font-semibold rounded-lg transition duration-75 hover:bg-magenta-100 hover:text-magenta-900 group"
              @click="toggleDropdown"
              :aria-expanded="dropdownOpen"
            >
              <span class="flex items-center gap-2"> All Jokes </span>
              <svg
                class="w-4 h-4 text-magenta-600"
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
                  class="block px-3 py-2 text-magenta-700 text-base rounded-lg hover:bg-magenta-100 hover:text-magenta-900"
                  @click="changeCategory('random')"
                >
                  Random Jokes
                </a>
              </li>
              <li>
                <a
                  class="block px-3 py-2 text-magenta-700 text-base rounded-lg hover:bg-magenta-100 hover:text-magenta-900"
                  @click="changeCategory('programming')"
                >
                  Programming Jokes
                </a>
              </li>
            </ul>
          </li>
          <li>
            <a
              class="flex items-center justify-between px-4 py-4 text-lg text-magenta-700 font-semibold rounded-lg transition duration-75 hover:bg-magenta-100 hover:text-magenta-900"
              @click="changeView('favorites')"
            >
              <span class="flex items-center gap-2 px-1"> Favorites</span>
              <span class="w-6 h-6 text-magenta-600">
                <svg
                  xmlns="http://www.w3.org/2000/svg"
                  fill="none"
                  viewBox="0 0 28 28"
                  stroke="currentColor"
                >
                  <path
                    stroke-linecap="round"
                    stroke-linejoin="round"
                    stroke-width="2"
                    d="M20.84 4.61a5.5 5.5 0 00-7.78 0L12 5.67l-1.06-1.06a5.5 5.5 0 00-7.78 7.78L12 21.23l8.84-8.84a5.5 5.5 0 000-7.78z"
                  />
                </svg>
              </span>
            </a>
          </li>
        </ul>
        <div class="fixed bottom-0 flex items-center gap-4 p-4 border-t border-gray-200 mt-8">
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
      <div class="flex justify-between items-center mb-6">
        <div
          v-if="activeView !== 'favorites'"
          class="flex bg-gray-200 rounded-full overflow-hidden shadow-sm"
        >
          <button
            class="px-3 py-1 text-sm text-gray-800 hover:bg-gray-300 transition"
            :class="{ 'bg-gray-300': activeCategory === 'random' }"
            @click="changeCategory('random')"
          >
            Random Jokes
          </button>
          <button
            class="px-3 py-1 text-sm text-gray-800 hover:bg-gray-300 transition"
            :class="{ 'bg-gray-300': activeCategory === 'programming' }"
            @click="changeCategory('programming')"
          >
            Programming Jokes
          </button>
        </div>
        <div v-if="activeView === 'favorites'" class="flex items-center"></div>
        <form class="relative w-1/2 max-w-sm">
          <div class="absolute inset-y-0 left-0 flex items-center pl-3 pointer-events-none">
            <svg
              class="w-4 h-4 text-gray-500 dark:text-gray-400"
              aria-hidden="true"
              xmlns="http://www.w3.org/2000/svg"
              fill="none"
              viewBox="0 0 20 20"
            >
              <path
                stroke="currentColor"
                stroke-linecap="round"
                stroke-linejoin="round"
                stroke-width="2"
                d="m19 19-4-4m0-7A7 7 0 1 1 1 8a7 7 0 0 1 14 0Z"
              />
            </svg>
          </div>
          <input
            type="search"
            class="block w-full py-2 pl-10 text-sm text-gray-900 border border-gray-300 rounded-lg bg-gray-50 focus:ring-gray-500 focus:border-gray-500"
            placeholder="Search jokes..."
            v-model="searchJoke"
          />
        </form>
      </div>
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
          <div class="flex items-center justify-between w-full">
            <div class="flex items-center">
              <h2 class="text-2xl font-bold tracking-tight text-gray-900 py-4 px-4">
                {{ HeaderJokeCards }}
              </h2>
            </div>
            <div class="relative inline-block w-32">
              <select
                v-model="selectedSort"
                class="appearance-none bg-gray-50 border border-gray-300 text-gray-900 text-sm rounded-md focus:ring-gray-500 focus:border-gray-500 block w-full p-1 pr-6"
              >
                <option value="Random">None</option>
                <option value="Rating">Rating</option>
                <option value="Alphabetically">Alphabetically</option>
              </select>
              <svg
                class="absolute right-1 top-1/2 transform -translate-y-1/2 w-4 h-4 text-gray-500 pointer-events-none"
                xmlns="http://www.w3.org/2000/svg"
                fill="none"
                viewBox="0 0 24 24"
                stroke="currentColor"
                stroke-width="2"
              >
                <path stroke-linecap="round" stroke-linejoin="round" d="M19 9l-7 7-7-7" />
              </svg>
            </div>
          </div>
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
              :settedRating="joke.rating || 0"
              @rate-joke="updateJokeRating"
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
      searchJoke: '',
      selectedSort: 'Random',
    }
  },

  methods: {
    async fetchJokes(forced = false) {
      try {
        this.spinner = true
        const storedJokes = localStorage.getItem('totalStoredJokes')
        const parsedJokes = JSON.parse(storedJokes)
        if (storedJokes && parsedJokes[this.activeCategory]?.length && !forced) {
          this.jokes = JSON.parse(storedJokes)
          this.favoriteJokes = Object.values(this.jokes)
            .flat()
            .filter((joke) => joke.isFavorite === true)
          return
        }
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
            rating: 0,
          })),
        ]
        localStorage.setItem('totalStoredJokes', JSON.stringify(this.jokes))
      } catch (error) {
        alert('Failed to load jokes. Please try to refresh your page.')
        console.error('Error fetching jokes:', error)
      } finally {
        this.spinner = false
      }
    },
    async showMoreJokes() {
      await this.fetchJokes(true)
    },
    scrollToTop() {
      window.scrollTo(0, 0) // TODO: Fix the navigation to the top of the page
    },
    changeCategory(category) {
      this.changeView('all')
      if (this.activeCategory === category) return
      this.activeCategory = category
      if (!this.jokes[category]?.length) this.fetchJokes(true)
    },
    changeView(view) {
      this.activeView = view
    },
    toggleFavorite(jokeId) {
      const jokeToUpdate = Object.values(this.jokes)
        .flat()
        .find((joke) => joke.id === jokeId)

      if (jokeToUpdate) {
        jokeToUpdate.isFavorite = !jokeToUpdate.isFavorite
        if (jokeToUpdate.isFavorite) {
          this.favoriteJokes.push(jokeToUpdate)
        } else {
          this.favoriteJokes = this.favoriteJokes.filter((joke) => joke.id !== jokeId)
        }
        localStorage.setItem('totalStoredJokes', JSON.stringify(this.jokes))
      }
    },
    toggleDropdown() {
      this.dropdownOpen = !this.dropdownOpen
    },
    updateJokeRating({ id, rating }) {
      const jokeToUpdate = Object.values(this.jokes)
        .flat()
        .find((joke) => joke.id === id)

      if (jokeToUpdate) {
        jokeToUpdate.rating = rating
        localStorage.setItem('totalStoredJokes', JSON.stringify(this.jokes))
      }
    },
  },
  created() {
    this.fetchJokes()
  },
  computed: {
    displayedJokes() {
      let jokes =
        this.activeView === 'favorites' ? this.favoriteJokes : this.jokes[this.activeCategory] || []
      // Search bar
      if (this.searchJoke) {
        jokes = jokes.filter((joke) =>
          joke.setup.toLowerCase().includes(this.searchJoke.toLowerCase()),
        )
      }
      // Sort picklist
      if (this.selectedSort === 'Rating') {
        // TODO: fix the rating
      } else if (this.selectedSort === 'Alphabetical') {
        jokes.sort((a, b) => a.setup.localeCompare(b.setup))
      }
      return jokes
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
