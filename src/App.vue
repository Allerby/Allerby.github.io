<template>
  <div class="mx-20">
    <div class="max-w-screen-lg mx-auto mt-20">
      <h1 class="text-3xl sm:text-5xl lg:text-6xl leading-none font-extrabold text-gray-900 tracking-tight mb-12">
        Projects
      </h1>
      <Suspense>
        <Cards />
      </Suspense>
    </div>
  </div>
</template>

<script>
import Cards from './components/cards.vue'

export default {
  components: {
    Cards
  },
}
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
}
</style>