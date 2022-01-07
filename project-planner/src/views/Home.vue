<template>
  <div class="home">
    <div v-if="projects.length">
      <div v-for="project in projects" :key="project.id">
        <SingleProject :project="project" />
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
};

// Other comments:
// Now we are going to create a new component that we will use for the project bar itself, to be reusable wherever we want.
// in " <SingleProject :project="project" />"  the ':project' is a prop we create to pass in the 'project' from the v-for loop (so we loop through projects and pass each individual project to the SingleProject component via the :project prop). We could have names ':project' whatever we want.

</script>
