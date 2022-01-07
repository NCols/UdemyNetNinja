<template>
  <div class="home">
    <div v-if="projects.length">
      <div v-for="project in projects" :key="project.id">
        <SingleProject :project="project" @deleteProj="handleDelete" />
      </div>
    </div>
  </div>
</template>

<script>
import SingleProject from "../components/SingleProject.vue"

export default {
  name: "Home",
  components: {
    SingleProject,
  },
  data() {
    return {
      projects: [],
    };
  },
  mounted() {
    fetch("http://localhost:3000/projects")
      .then((res) => res.json()) 
      .then((data) => (this.projects = data))
      .catch((err) => console.log(err.message));
  },
  methods: {
    handleDelete(id) {  // The id that we pass here comes from SingleProject.vue "then(() => this.$emit('deleteProj', this.project.id))"
      this.projects = this.projects.filter((project) => {
        return project.id != id  // Keep the projects which id's are not the one we want to delete.
      })
    }
  }
};

// Other comments:
// Now we are going to create a new component that we will use for the project bar itself, to be reusable wherever we want.
// in " <SingleProject :project="project" />"  the ':project' is a prop we create to pass in the 'project' from the v-for loop (so we loop through projects and pass each individual project to the SingleProject component via the :project prop). We could have names ':project' whatever we want.

</script>
