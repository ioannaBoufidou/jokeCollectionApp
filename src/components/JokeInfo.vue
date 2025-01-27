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
          v-if="!isRevealed"
          class="absolute top-2 right-2 rounded-full w-8 h-8 items-center justify-center text-gray-800 hover:bg-gray-300 z-10 border border-gray-400"
          @click.stop="toggleFavorite"
        >
          {{ isFavorite ? 'x' : '+' }}
        </button>
        <h2 class="text-lg font-bold text-gray-800">{{ setup }}</h2>
        <p class="mt-4 text-gray-500 italic">Click to reveal the punchline</p>
      </div>
      <!-- back side of the card -->
      <div
        class="bg-gray-100 rounded-2xl shadow-lg hover:shadow-2xl p-6 flex flex-col justify-center absolute inset-0 card-back"
      >
        <button
          v-if="isRevealed"
          class="absolute top-2 right-2 rounded-full w-8 h-8 items-center justify-center text-gray-800 hover:bg-gray-300 z-10 border border-gray-400"
          @click.stop="toggleFavorite"
        >
          {{ isFavorite ? 'x' : '+' }}
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
