<template>
  <div class="home">
    <FilterNav @filterChange="current = $event" :current="current" />
    <div v-if="projects.length">
      <div v-for="project in filteredProjects" :key="project.id">
        <SingleProject :project="project" @deleteProj="handleDelete" @completeProj="handleComplete" />
      </div>
    </div>
  </div>
</template>

<script>
import SingleProject from "../components/SingleProject.vue"
import FilterNav from "../components/FilterNav.vue"

export default {
  name: "Home",
  components: {
    SingleProject,
    FilterNav,
  },
  data() {
    return {
      projects: [],
      current: 'all',
      
    };
  },
  mounted() {
    fetch("http://localhost:3000/projects")
      .then((res) => res.json()) 
      .then((data) => (this.projects = data))
      .catch((err) => console.log(err.message));
  },
  methods: {
    handleDelete(id) {  
      this.projects = this.projects.filter((project) => {
        return project.id != id  
      })
    },
    handleComplete(id) {
      let proj = this.projects.find(project => {
        return project.id === id  // Return true if the 'id' (passed param) matches project.id for every project, if true, return and assign to 'proj' variable. This way we look for the project we need, based on id, we store it in proj.
      })
      proj.complete = !proj.complete  // Update the 'complete' property for that particular project.
    }
  },
  computed: {
    filteredProjects() {
      if (this.current === 'completed') {
        return this.projects.filter(project => project.complete)
      }
      if (this.current === 'ongoing') {
        return this.projects.filter(project => !project.complete)
      }
      return this.projects

    }
  }
};

// Other comments:
// Now we are going to create a new component that we will use for the project bar itself, to be reusable wherever we want.
// in " <SingleProject :project="project" />"  the ':project' is a prop we create to pass in the 'project' from the v-for loop (so we loop through projects and pass each individual project to the SingleProject component via the :project prop). We could have names ':project' whatever we want.

// <FilterNav @filterChange="current = $event" />
// We listen for the event 'filterChange' that's emitted from FilterNav. The '$event' is whatever data we passed as 'status' from FilterNav.
</script>
