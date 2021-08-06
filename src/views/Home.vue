<template>
  <div class="home">
<FilterNav @filterChange="current=$event" :current="current"/>
  </div>
  <div v-if="projects">
    <div v-for="project in filteredProjects" :key="project.id">
      <SingleProject @delete="handleDelete" @complete="handleComplete"  :project="project"/>
    </div>
  </div>
  <div v-else>
    Loading 
  </div>
</template>

<script>
import FilterNav from '../components/FilterNav.vue'
import SingleProject from '@/components/SingleProject.vue'
// @ is an alias to /src


export default {
  name: 'Home',
  components: { SingleProject, FilterNav
  },
  data() {
    return {
      projects: [],
      current:'all'
    }
  },
  methods: {
    handleDelete(id) {
      this.projects=this.projects.filter((project)=>{
        return project.id!==id
      })
      
    },
    handleComplete(id){
      let p = this.projects.find((project)=>{
        return project.id===id
      })
      p.complete=!p.complete
    }
  },
  mounted () {
    fetch('http://localhost:3000/projects')
    .then(res=>res.json())
    .then(data=>this.projects=data)
    .catch(err=>console.log(err.message));
  },
  computed: {
      filteredProjects() {
        if(this.current=='all')
          return this.projects 
        else if(this.current=='completed')
         return this.projects.filter((project)=>{
            return project.complete
         })
         else return this.projects.filter((project)=>{
            return !project.complete
         })

      }
  },
}
</script>
