<template>
  <form @submit.prevent="handleSubmitEdit">
      <label>Title: </label>
      <input type="text" v-model="title" required>
      <label>Details: </label>
      <textarea v-model="details" required></textarea>
      <button>Update Project</button>
  </form>
</template>

<script>
export default {
    props: ['id'],
    data() {
        return {
            title: '',
            details: '',
            uri: 'http://localhost:3000/projects/' + this.id  // 'this' references id, then '.id' references the id prop that we imported
        }
    },
    mounted() {
        // Fetch the data of this project, convert response to json to data, update the values of the title and details properties -> populates the fields so that we can edit the project
        fetch(this.uri)
            .then(res => res.json())
            .then(data => {
                this.title = data.title
                this.details = data.details
            })
    },
    methods: {
        handleSubmitEdit(id) {
            let project = {
              title: this.title,
              details: this.details,
              complete: false
          }
          
          fetch(this.uri, {
              method: 'PATCH',
              headers: { 'Content-Type': 'application/json'},
              body: JSON.stringify(project) 
          })
            .then(() => {
                this.$router.push('/')
            })
            .catch(err => console.log(err.message))
        }
    }
}
</script>

<style>

</style>