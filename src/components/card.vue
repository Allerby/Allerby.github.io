<template>
  <div class="relative py-10 bg-white shadow-md rounded-3xl p-12 bg-gradient-to-r from-indigo-400 to-purple-600 text-white">
    <h3 class="text-3xl tracking-tight font-extrabold mb-8">{{ project.name }}</h3>
    <p class="">{{ project.description }}</p>
  </div>
</template>

<script>
  import { defineComponent } from 'vue'

  export default defineComponent({
    props: {
      project: {
        type: Object,
        required: true,
      },
    },
  })
</script>

<style scoped>
</style>