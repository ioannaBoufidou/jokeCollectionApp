<template>
  <div class="card-container relative w-full h-full" @click="onRevealClick">
    <div class="card-inner duration-500" :class="{ 'rotate-y-180': isRevealed }">
      <!-- front side of the card -->
      <div
        class="bg-white rounded-2xl shadow-lg hover:shadow-2xl p-6 flex flex-col justify-between h-64"
      >
        <h2 class="text-lg font-bold text-gray-800">{{ setup }}</h2>
        <p class="mt-4 text-gray-500 italic">Click to reveal the punchline</p>
      </div>
      <!-- back side of the card-- TODO: change the gray color maybe -->
      <div
        class="bg-gray-100 rounded-2xl shadow-lg hover:shadow-2xl p-6 flex items-center justify-center absolute inset-0 card-back"
      >
        <p class="text-gray-800">{{ punchline }}</p>
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
  },
  data() {
    return { isRevealed: false }
  },
  methods: {
    onRevealClick() {
      this.isRevealed = !this.isRevealed
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
