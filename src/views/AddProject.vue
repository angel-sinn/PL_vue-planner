<template>
  <form @submit.prevent="handleSubmit">
    <label>Title:</label>
    <input type="text" v-model="title" required>
    <label>Details:</label>
    <textarea required v-model="details"></textarea>
    <button>Add To List</button>
  </form>
</template>

<script>
export default {
  data() {
    return {
      title: '',
      details: '',
    }
  },
  methods: {
    handleSubmit() {
      // console.log(this.title, this.details);
      let project = {
        // json-server will add unique id automatically
        title: this.title,
        details: this.details,
        complete: false
      }
      fetch('http://localhost:3000/projects', {
        method: 'POST',
        headers: { 'Content-Type': 'application/json'},
        body: JSON.stringify(project)
      }).then(() => {
        // redirect to homepage
        this.$router.push('/')
      }).catch(err => console.log(err.message))
    }
  }
}
</script>

<style>
  form {
    background: white;
    padding: 20px;
    border-radius: 10px;
  }
  label {
    display: block;
    color: #bbb;
    text-transform: uppercase;
    font-size: 14px;
    font-weight: bold;
    letter-spacing: 1px;
    margin: 20px 0 10px 0;
  }
  input {
     padding: 10px;
     border: 0;
     border-bottom: 1px solid #ddd;
     width: 100%;
     box-sizing: border-box;
  }
  textarea{
    border: 1px solid #ddd;
    padding: 10px;
    width: 100%;
    box-sizing: border-box;
    height: 100px;
  }
  form button {
    display: block;
    margin: 20px auto 0;
    background: rgb(79, 131, 228);
    color: white;
    padding: 10px;
    border: 0;
    border-radius: 6px;
    font-size: 16px;
  }
</style>