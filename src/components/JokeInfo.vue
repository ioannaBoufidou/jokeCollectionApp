<template>
  <div
    class="card bg-white rounded-2xl shadow-lg hover:shadow-2xl transition p-6 flex flex-col justify-between h-64"
    @click="handleClick"
  >
    <h2 class="text-lg font-bold text-gray-800">{{ setup }}</h2>
    <p class="mt-2 text-gray-600">{{ type }}</p>
    <p class="mt-4 text-gray-500 italic">Click to reveal the punchline</p>
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
  methods: {
    handleClick() {
      console.log(`Punchline: ${this.punchline}`)
    },
  },
}
</script>

<style>
.box {
  background-color: #5e31644d;
}

.box:hover {
  background-color: #e0e0e0;
  transform: scale(1.05);
}
</style>
