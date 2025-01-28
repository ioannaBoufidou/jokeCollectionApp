<template>
  <div class="card-container relative w-full h-full">
    <div
      class="card-inner duration-500"
      :class="{ 'rotate-y-180': isRevealed }"
      @click="onRevealClick"
    >
      <!-- front side of the card -->
      <div
        class="bg-white rounded-2xl shadow-lg hover:shadow-2xl p-6 flex flex-col justify-between h-64"
      >
        <button
          class="absolute top-4 right-4 rounded-full w-9 h-9 flex items-center justify-center bg-gray-100 hover:bg-red-100"
          @click.stop="toggleFavorite"
        >
          <svg
            v-if="!isFavorite"
            heart
            xmlns="http://www.w3.org/2000/svg"
            class="w-5 h-5 text-gray-500"
            fill="none"
            stroke="currentColor"
            stroke-width="2"
            viewBox="0 0 24 24"
          >
            <path
              stroke-linecap="round"
              stroke-linejoin="round"
              d="M12 21C12 21 3 13.625 3 8.5C3 5.739 5.239 3.5 8 3.5C9.657 3.5 11.173 4.405 12 5.708C12.827 4.405 14.343 3.5 16 3.5C18.761 3.5 21 5.739 21 8.5C21 13.625 12 21 12 21Z"
            />
          </svg>
          <svg
            v-else
            xmlns="http://www.w3.org/2000/svg"
            class="w-5 h-5 text-red-500"
            fill="currentColor"
            viewBox="0 0 24 24"
          >
            <path
              d="M12 21.35l-1.45-1.32C5.4 15.36 2 12.28 2 8.5 2 5.42 4.42 3 7.5 3c1.74 0 3.41.81 4.5 2.09C13.09 3.81 14.76 3 16.5 3 19.58 3 22 5.42 22 8.5c0 3.78-3.4 6.86-8.55 11.54L12 21.35z"
            />
          </svg>
        </button>
        <div style="margin-top: 25px">
          <h2 class="text-lg font-bold text-gray-800">{{ setup }}</h2>
        </div>
        <p class="mt-4 text-gray-500 italic">Click to reveal the punchline</p>
      </div>
      <!-- back side of the card -->
      <div
        class="bg-gray-100 rounded-2xl shadow-lg hover:shadow-2xl p-6 flex flex-col justify-center absolute inset-0 card-back"
      >
        <button
          class="absolute top-4 right-4 rounded-full w-9 h-9 flex items-center justify-center bg-gray-100 hover:bg-red-100"
          @click.stop="toggleFavorite"
        >
          <svg
            v-if="!isFavorite"
            heart
            xmlns="http://www.w3.org/2000/svg"
            class="w-5 h-5 text-gray-500"
            fill="none"
            stroke="currentColor"
            stroke-width="2"
            viewBox="0 0 24 24"
          >
            <path
              stroke-linecap="round"
              stroke-linejoin="round"
              d="M12 21C12 21 3 13.625 3 8.5C3 5.739 5.239 3.5 8 3.5C9.657 3.5 11.173 4.405 12 5.708C12.827 4.405 14.343 3.5 16 3.5C18.761 3.5 21 5.739 21 8.5C21 13.625 12 21 12 21Z"
            />
          </svg>
          <svg
            v-else
            xmlns="http://www.w3.org/2000/svg"
            class="w-5 h-5 text-red-500"
            fill="currentColor"
            viewBox="0 0 24 24"
          >
            <path
              d="M12 21.35l-1.45-1.32C5.4 15.36 2 12.28 2 8.5 2 5.42 4.42 3 7.5 3c1.74 0 3.41.81 4.5 2.09C13.09 3.81 14.76 3 16.5 3 19.58 3 22 5.42 22 8.5c0 3.78-3.4 6.86-8.55 11.54L12 21.35z"
            />
          </svg>
        </button>
        <p class="text-gray-800 text-center mb-4">{{ punchline }}</p>
        <div class="flex justify-center items-center">
          <svg
            v-for="star in 5"
            :key="star"
            :class="
              star <= currentRating
                ? 'w-6 h-6 cursor-pointer text-yellow-400'
                : 'w-6 h-6 cursor-pointer text-gray-400'
            "
            xmlns="http://www.w3.org/2000/svg"
            fill="currentColor"
            viewBox="0 0 22 20"
            @click.stop="rateJoke(star)"
          >
            <path
              d="M20.924 7.625a1.523 1.523 0 0 0-1.238-1.044l-5.051-.734-2.259-4.577a1.534 1.534 0 0 0-2.752 0L7.365 5.847l-5.051.734A1.535 1.535 0 0 0 1.463 9.2l3.656 3.563-.863 5.031a1.532 1.532 0 0 0 2.226 1.616L11 17.033l4.518 2.375a1.534 1.534 0 0 0 2.226-1.617l-.863-5.03L20.537 9.2a1.523 1.523 0 0 0 .387-1.575Z"
            />
          </svg>
          <p class="px-3 text-gray-500 mt-2 text-sm text-center">{{ currentRating }} out of 5</p>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  name: 'JokeInfo',
  props: {
    type: {
      type: String,
      required: true,
    },
    setup: {
      type: String,
      required: true,
    },
    punchline: {
      type: String,
      required: true,
    },
    id: {
      type: Number,
      required: true,
    },
    settedRating: {
      type: Number,
      default: 0,
    },
  },
  data() {
    return {
      isRevealed: false,
      isFavorite: false,
      currentRating: this.settedRating,
    }
  },
  methods: {
    onRevealClick() {
      this.isRevealed = !this.isRevealed
    },
    toggleFavorite() {
      this.isFavorite = !this.isFavorite
      this.$emit('toggle-favorite', this.id)
    },
    rateJoke(rating) {
      this.currentRating = rating
      this.$emit('rate-joke', { id: this.id, rating })
    },
  },
}
</script>

<style scoped>
.card-container {
  perspective: 1000px; /* depth to 3D effect */
  width: 100%;
  height: 100%;
}

.card-inner {
  transform-style: preserve-3d; /* 3D perspective */
}

.card-inner.rotate-y-180 {
  transform: rotateY(180deg);
}

.card-back {
  backface-visibility: hidden;
  transform: rotateY(180deg);
}

@keyframes cardFlip {
  0% {
    transform: rotateY(0deg);
  }
  50% {
    transform: rotateY(90deg) scale(1.1);
  }
  100% {
    transform: rotateY(180deg);
  }
}
</style>
