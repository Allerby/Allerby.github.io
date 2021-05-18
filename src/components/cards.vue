<template>
  <div class="grid grid-flow-row grid-cols-1 md:grid-cols-2 gap-8">
    <div 
      class='drop-zone p-2 bg-gray-200 rounded-3xl pb-0' 
      @drop='onDrop($event, 1)'
      @dragover.prevent
      @dragenter.prevent>
      <div 
        v-for='project in getList(1)' 
        :key='project.id' 
      >
        <Card 
          :project="project" 
          draggable=true
          @dragstart='startDrag($event, project)' 
        />
      </div>
    </div>

    <div 
      class='drop-zone p-2 bg-gray-200 rounded-3xl pb-0' 
      @drop='onDrop($event, 2)'
      @dragover.prevent
      @dragenter.prevent>
      <div 
        v-for='project in getList(2)' 
        :key='project.id' 
        >
        <Card 
          :project="project" 
          draggable=true
          @dragstart='startDrag($event, project)'
        />
      </div>
    </div>
    
  </div>
</template>

<script>
import Card from './card.vue'
import { supabase } from '../lib/supabase'
import { defineComponent, ref } from 'vue'

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

async function updateProjectList(project) {
  try {
    const { error } = await supabase
      .from('projects')
      .update({ list: project.list })
      .eq('id', project.id)
      .single()

    if (error) {
      alert(error.message)
      console.error('There was an error updating', error)
      return
    }

    console.log('Updated project', project.id)
  } catch (err) {
    alert('Error')
    console.error('Unknown problem updating record', err)
  }
}

export default defineComponent({
  name: 'Cards',
  components: {
    Card
  },
  async setup() {
    let projects = await getProjects(); 
    projects = ref(projects)

    const getList = (list) => {
      return projects.value.filter(project => project.list === list)
    }
    
    return {
      projects,
      getList,
    }
  },
  methods: {
    startDrag: (event, item) => {
      event.dataTransfer.dropEffect = 'move'
      event.dataTransfer.effectAllowed = 'move'
      event.dataTransfer.setData('itemID', item.id)
    },
    onDrop(event, list) {
      const itemID = event.dataTransfer.getData('itemID')
      const project = this.projects.find(project => project.id == itemID)
      project.list = list
      updateProjectList(project)
    }
  }
})
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
}
</style>