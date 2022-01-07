<template>
  <div class="home">
    <div v-if="projects.length">
      <div v-for="project in projects" :key="project.id">
        <p>{{ project.title }}</p>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  name: "Home",
  components: {},
  data() {
    return {
      projects: [],
    };
  },
  mounted() {
    fetch("http://localhost:3000/projects")
      .then((res) => res.json()) // Take the response, pass through json()
      .then((data) => (this.projects = data)) // Take the json data returned and assign to 'projects' property
      .catch((err) => console.log(err.message));
  },
};

// Other comments:
// v-if="projects.length"  =>  show element only if projects.length is true => if projects is not empty
</script>
