<template>
  <div class="project">
    <div class="actions">
      <h3 @click="showDetails = !showDetails">{{ project.title }}</h3>
      <div class="icons">
        <span class="material-icons">edit</span>
        <span @click="deleteProject" class="material-icons">delete</span>
        <span class="material-icons">done</span>
      </div>
    </div>
    <div v-if="showDetails" class="details">
      <p>{{ project.details }}</p>
    </div>
  </div>
</template>

<script>
export default {
  props: ["project"],
  data() {
    return {
      showDetails: false,
      uri: 'http://localhost:3000/projects/' + this.project.id  // JSON API endpoint to delete an individual project
    };
  },
  methods: {
      deleteProject() {
          fetch(this.uri, { method: 'DELETE' }) // Makes a DELETE request to the JSON server at the 'uri' endpoint. 
            .then(() => this.$emit('deleteProj', this.project.id)) // After emitting this custom event, we now must listen to it in our 'Home.vue' component. This event will allow us to keep the local 'projects' array from Home.vue aligned with the db. In this case, if we click delete, then the item should disappear from db (line above) and from the screen, so we update the projects property as well.
            .catch(err => console.log(err.message))
      }
  },
};
</script>

<style>
.project {
  margin: 20px auto;
  background: white;
  padding: 10px 20px;
  border-radius: 4px;
  box-shadow: 1px 2px 3px rgba(0, 0, 0, 0.5);
  border-left: 4px solid #e90074;
}
h3 {
  cursor: pointer;
}
.actions {
  display: flex;
  justify-content: space-between;
  align-items: center;
}
.material-icons {
    font-size: 24px;
    margin-left: 10px;
    color: #bbb;
    cursor: pointer;
}
.material-icons:hover {
    color: #777;
}
</style>