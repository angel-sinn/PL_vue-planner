<template>
  <div class="project" :class="{ complete: project.complete }">
    <div class="actions">
      <h3 @click="toggleDetails">{{project.title}}</h3>
      <div class="icons">
        <span class="material-icons">edit</span>
        <span @click="deleteProject" class="material-icons">delete</span>
        <span @click="toggleComplete" class="material-icons tick">done</span>
      </div>
    </div>
    <div v-if="showDetails" class="details">
      <p>{{project.details}}</p>
    </div>
  </div>
</template>

<script>
export default {
  props: ['project'],
  data() {
    return {
      showDetails: false,
      uri: 'http://localhost:3000/projects/' + this.project.id
    }
  },
  methods: {
    toggleDetails() {
      this.showDetails = !this.showDetails
    },
    deleteProject() {
      fetch(this.uri, {method: 'DELETE'})
      // emit delete and attach project id
        .then(() => this.$emit('delete', this.project.id))
        .catch(err => console.log(err.message))
    },
    toggleComplete() {
      fetch(this.uri, {
        method: 'PATCH',
        headers: { 'Content-Type': 'application/json'},
        // send data via JSON string from client to server, vice versa
        body: JSON.stringify({complete: !this.project.complete})
      }).then (() => {
        // emit event so that local data is in sync
        this.$emit('complete', this.project.id)
      }).catch(err => console.log(err.message))
    }
  }
}
</script>

<style>
  .project {
    margin: 20px auto;
    background: white;
    padding: 10px 20px;
    border-radius: 4px;
    box-shadow: 1px 2px 3px rgba(0,0,0,0.05);
    border-left: 4px solid crimson;
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
  /* completed projects */
  .project.complete {
    border-left: 4px solid rgb(8, 184, 108);
  }
  .project.complete .tick {
    color: rgb(8, 184, 108);
  }
</style>