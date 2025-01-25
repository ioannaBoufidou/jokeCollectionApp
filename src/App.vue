<template>
  <div id="app" class="flex h-screen">
    <aside id="separator-sidebar" class="w-80 h-full bg-gray-50" aria-label="Sidebar">
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
    <main class="flex-1 bg-gray-100 p-6">
      <header class="flex justify-center mb-8">
        <div class="flex bg-gray-200 rounded-lg overflow-hidden">
          <button
            class="px-4 py-2 text-gray-800 hover:bg-gray-300 transition"
            :class="{ 'bg-gray-300': activeCategory === 'Random jokes' }"
            @click="activeCategory = 'Random jokes'"
          >
            Random jokes
          </button>
          <button
            class="px-4 py-2 text-gray-800 hover:bg-gray-300 transition"
            :class="{ 'bg-gray-300': activeCategory === 'programming jokes' }"
            @click="activeCategory = 'programming jokes'"
          >
            Programming jokes
          </button>
        </div>
      </header>
      <div class="grid grid-cols-3 gap-8 mx-auto justify-center">
        <JokeInfo
          v-for="box in boxes"
          :key="box.id"
          :type="box.type"
          :setup="box.setup"
          :punchline="box.punchline"
        />
      </div>
    </main>
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
      activeCategory: 'Random jokes',
      boxes: [
        {
          type: 'general',
          setup: 'What’s the difference between an African elephant and an Indian elephant?',
          punchline: 'About 5000 miles.',
          id: 279,
        },
        {
          type: 'general',
          setup: 'What do you call a pig that knows karate?',
          punchline: 'A pork chop!',
          id: 212,
        },
        {
          type: 'programming',
          setup: "What's the best thing about a Boolean?",
          punchline: "Even if you're wrong, you're only off by a bit.",
          id: 15,
        },
        {
          type: 'general',
          setup: 'Why do birds fly south for the winter?',
          punchline: "Because it's too far to walk.",
          id: 344,
        },
        {
          type: 'programming',
          setup: 'Why do programmers prefer dark chocolate?',
          punchline: "Because it's bitter like their code.",
          id: 416,
        },
      ],
    }
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
