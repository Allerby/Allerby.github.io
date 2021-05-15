<template>
  <div class="grid grid-flow-row grid-cols-1 md:grid-cols-2 gap-8">
    <Card v-for="project in projects" :key="project.id" :project="project" />
  </div>
</template>

<script>
import Card from './card.vue'
import { supabase } from '../lib/supabase'
import { defineComponent } from 'vue'

async function getProjects() {
  try {
    const { data: projects, error } = await supabase
      .from('projects')
      .select('*')

    if (error) {
      console.log('error:', error)
      return
    }
    // handle for when no projects are returned
    if (projects === null) {
      console.log('error:', 'no projects')
      return
    }
    return projects
  } catch (err) {
    console.error('Error retrieving data from db', err)
  }
}

export default defineComponent({
  name: 'Cards',
  components: {
    Card
  },
  async setup() {
    let projects = await getProjects(); 
    
    return {
      projects: projects,
    }
  }
})


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