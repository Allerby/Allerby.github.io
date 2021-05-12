<template>
  <div class="max-w-screen-lg mx-auto mt-20">
    <h1 class="text-3xl sm:text-5xl lg:text-6xl leading-none font-extrabold text-gray-900 tracking-tight mb-12">
      Projects
    </h1>
    <div class="grid grid-flow-col grid-cols-2 gap-8">
      <Card v-for="project in projects" :key="project.id" :project="project" />
    </div>
  </div>
</template>

<script>
import Card from './components/card.vue'
import { supabase } from './lib/supabase'

let { data: projects, error } = await supabase
  .from('projects')
  .select('*')

export default {
  components: {
    Card
  },
  setup() {
    return {
      projects: projects,
    }
  }
}


// This starter template is using Vue 3 experimental <script setup> SFCs
// Check out https://github.com/vuejs/rfcs/blob/script-setup-2/active-rfcs/0000-script-setup.md
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
}
</style>