<template>
  <div class="home">
    <!-- $event is to get data that is being sent up -->
    <FilterNav @filterChange="current = $event" :current="current"/>
    <div v-if="projects.length">
      <div v-for="project of filteredProjects" :key="project.id">
        <SingleProject :project="project" @delete="handleDelete" @complete="handleComplete"/>
      </div>
    </div>
  </div>
</template>

<script>
import SingleProject from '../components/SingleProject.vue'
import FilterNav from '../components/FilterNav.vue'

export default {
  name: 'Home',
  components: {SingleProject, FilterNav},
  data() {
    return {
      projects: [],
      current: 'all'
    }
  },
  mounted() {
    fetch('http://localhost:3000/projects')
      .then(res => res.json())
      .then(data => this.projects = data)
      .catch(err => console.log(err.message))
  },
  methods: {
    // make sure local data is up to date
    handleDelete(id) {
      this.projects = this.projects.filter((project) => {
        return project.id !== id
      })
    },
    // make sure local data is up to date
    handleComplete(id) {
      let p = this.projects.find((project) => {
        return project.id === id
      })
      // update status
      p.complete = !p.complete
    }
  },
  computed: {
    filteredProjects() {
      if (this.current === 'completed') {
        return this.projects.filter((project) => project.complete)
      } 
      if (this.current === 'ongoing') {
        return this.projects.filter((project) => !project.complete)
      }
      return this.projects
    }
  }
}
</script>
