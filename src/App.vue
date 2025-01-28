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
              class="flex items-center justify-between w-full px-2 py-4 text-lg text-fuchsia-900 font-semibold rounded-lg transition group"
              @click="toggleDropdown"
              :aria-expanded="dropdownOpen"
            >
              <span class="flex items-center gap-2 text-fuchsia-900"> All Jokes </span>
              <svg
                class="w-4 h-4 text-gray-900"
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
                  class="block px-3 py-2 text-gray-700 text-base rounded-lg hover:bg-gray-100 hover:text-gray-900"
                  @click="changeCategory('random')"
                >
                  Random Jokes
                </a>
              </li>
              <li>
                <a
                  class="block px-3 py-2 text-gray-700 text-base rounded-lg hover:bg-gray-100 hover:text-gray-900"
                  @click="changeCategory('programming')"
                >
                  Programming Jokes
                </a>
              </li>
            </ul>
          </li>
          <li>
            <a
              class="flex items-center justify-between px-4 py-4 text-lg text-fuchsia-900 font-semibold rounded-lg transition duration-75 hover:bg-fuchsia-900 hover:text-fuchsia-900"
              @click="changeView('favorites')"
            >
              <span class="flex items-center gap-2 px-1"> Favorites</span>
              <span class="w-6 h-6 text-gray-900">
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
          <li>
            <a
              class="flex items-center justify-between px-4 py-4 text-lg text-gray-700 font-semibold rounded-lg transition duration-75 hover:bg-gray-100 hover:text-gray-900"
              @click="isModalOpen = true"
            >
              <span class="flex items-center gap-2 px-1">App Info/Statistics</span>
              <span class="w-6 h-6 text-fuchsia-900">
                <svg
                  xmlns="http://www.w3.org/2000/svg"
                  fill="none"
                  viewBox="0 0 24 24"
                  stroke="currentColor"
                  class="w-6 h-6 text-gray-900"
                >
                  <path
                    stroke-linecap="round"
                    stroke-linejoin="round"
                    stroke-width="2"
                    d="M12 8v4m0 4h.01M21 12a9 9 0 11-18 0 9 9 0 0118 0z"
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
      <div id="topToScroll" class="flex justify-between items-center mb-6">
        <div
          v-if="activeView !== 'favorites'"
          class="flex bg-gray-200 rounded-full overflow-hidden shadow-sm"
          :class="{ 'opacity-50 pointer-events-none': spinner }"
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
            :class="{ 'opacity-50 pointer-events-none': spinner }"
          />
        </form>
      </div>
      <div v-if="spinner" class="flex items-center justify-center bg-opacity-70 py-50">
        <div class="flex flex-col items-center text-center text-gray-700">
          <div class="animate-spin h-8 w-8 border-4 border-t-transparent rounded-full"></div>
          <span class="mt-4 text-lg"
            >Fetching jokes... Your daily dose of laughter is on its way!</span
          >
        </div>
      </div>
      <div v-else>
        <div>
          <div class="flex items-center justify-between w-full py-4">
            <div class="flex items-center">
              <h2 class="text-2xl font-bold tracking-tight text-gray-900 py-4 px-4">
                {{ HeaderJokeCards }}
              </h2>
            </div>
            <div class="flex gap-2">
              <div class="relative inline-block w-32">
                <label class="block text-xs font-medium text-gray-500 px-1">Sort by</label>
                <div class="relative">
                  <select
                    v-model="selectedSort"
                    class="appearance-none bg-white border border-gray-300 text-gray-900 text-sm rounded-lg focus:ring-gray-500 focus:border-gray-500 block w-full p-2 pr-8 shadow-sm"
                  >
                    <option value="Random">None</option>
                    <option value="Rating">Rating</option>
                    <option value="Alphabetically">Alphabetically</option>
                  </select>
                  <svg
                    class="absolute right-3 top-1/2 transform -translate-y-1/2 w-4 h-4 text-gray-500 pointer-events-none"
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
              <div class="relative inline-block w-32">
                <label class="block text-xs font-medium text-gray-500 px-1">Filter by</label>
                <div class="relative">
                  <select
                    v-model="selectedFilter"
                    class="appearance-none bg-white border border-gray-300 text-gray-900 text-sm rounded-lg focus:ring-gray-500 focus:border-gray-500 block w-full p-2 pr-8 shadow-sm"
                  >
                    <option value="0">None</option>
                    <option value="1">⭐</option>
                    <option value="2">⭐⭐</option>
                    <option value="3">⭐⭐⭐</option>
                    <option value="4">⭐⭐⭐⭐</option>
                    <option value="5">⭐⭐⭐⭐⭐</option>
                  </select>
                  <svg
                    class="absolute right-3 top-1/2 transform -translate-y-1/2 w-4 h-4 text-gray-500 pointer-events-none"
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
            </div>
          </div>
          <div
            v-if="activeView === 'favorites' && displayedJokes.length === 0"
            class="flex flex-col items-center justify-center h-full py-50"
          >
            <span
              >Looks like your Favorites list could use a boost. Go back to All Jokes to add some
              jokes to your Favorites list.</span
            >
          </div>
          <div
            v-if="displayedJokes.length === 0 && (selectedFilter > 0 || searchJoke !== '')"
            class="flex flex-col items-center justify-center h-full py-50"
          >
            <span>Oooops!😪</span>
            <span v-if="selectedFilter > 0"
              >There are no jokes with {{ selectedFilter }} star rating. You can try search
              something else</span
            >
            <span v-else="searchJoke !== ''"
              >There are no jokes with word '{{ searchJoke }}'. You can try search something
              else</span
            >
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
        <div
          v-if="activeView !== 'favorites' && searchJoke === '' && selectedFilter === 0"
          class="flex justify-center mt-6"
        >
          <button
            class="text-fuchsia-900 hover:text-gray-600 underline text-lg"
            @click="showMoreJokes"
          >
            Show more
          </button>
        </div>
      </div>
    </main>
    <button
      class="fixed bottom-4 right-4 p-3 rounded-full bg-fuchsia-900 text-white hover:bg-gray-600"
      @click="scrollToTop"
    >
      ^
    </button>
  </div>
  <div
    v-if="isModalOpen"
    class="fixed inset-0 flex items-center justify-center z-50 modal-overlay"
    @click.self="isModalOpen = false"
  >
    <div class="bg-white rounded-lg shadow-lg p-8 max-w-md w-full">
      <div class="text-center mb-6">
        <h2 class="text-2xl font-bold text-gray-800">Collection Statistics</h2>
        <p class="text-gray-600">Here are some insights about your collection:</p>
      </div>
      <div class="grid grid-cols-2 gap-4 text-center py-4">
        <div class="bg-gray-50 rounded-lg p-4 shadow-sm">
          <p class="text-sm text-gray-500">Total Favorite Jokes</p>
          <p class="text-2xl font-semibold text-fuchsia-900">{{ collectionStatistics().total }}</p>
        </div>
        <div class="bg-gray-50 rounded-lg p-4 shadow-sm">
          <p class="text-sm text-gray-500">Average Rating</p>
          <div class="inline-flex items-center">
            <p class="text-2xl font-semibold text-fuchsia-900">
              {{ collectionStatistics().average }}
            </p>
            <svg
              class="w-6 h-6 text-yellow-400 ml-2"
              xmlns="http://www.w3.org/2000/svg"
              fill="currentColor"
              viewBox="0 0 24 24"
            >
              <path
                d="M12 2.5l3.09 6.26L22 9.34l-5 4.87 1.18 6.89L12 17.77l-6.18 3.33L7 14.21 2 9.34l6.91-1.58L12 2.5z"
              />
            </svg>
          </div>
        </div>
      </div>
      <div class="mt-6 text-center py-4">
        <button
          @click="isModalOpen = false"
          class="bg-gray-500 text-white px-6 py-2 rounded-lg shadow hover:bg-gray-600 transition"
        >
          Close
        </button>
      </div>
    </div>
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
      selectedFilter: 0,
      isModalOpen: false,
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
        const data = await response.json()
        const newJokes = data.filter(
          (newJoke) =>
            !Object.values(this.jokes)
              .flat()
              .some((existingJoke) => existingJoke.id === newJoke.id),
        )
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
      document.getElementById('topToScroll').scrollIntoView()
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
    collectionStatistics() {
      const total = this.favoriteJokes.length
      let totalRatings = 0
      let totalCount = 0

      for (const category in this.jokes) {
        this.jokes[category].forEach((joke) => {
          totalRatings += parseInt(joke.rating)
          totalCount++
        })
      }
      const average = totalCount > 0 ? (totalRatings / totalCount).toFixed(1) : 0
      return { total, average }
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
        jokes.sort((a, b) => b.rating - a.rating)
      } else if (this.selectedSort === 'Alphabetically') {
        jokes.sort((a, b) => a.setup.localeCompare(b.setup))
      }
      // filter picklist
      if (this.selectedFilter > 0) {
        jokes = jokes.filter((j) => j.rating == this.selectedFilter)
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

.modal-overlay {
  background-color: rgba(87, 84, 84, 0.5);
}
</style>
