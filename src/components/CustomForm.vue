<template>
   <div>
      <form action="post" @submit="checkForm">        
        <p >
          <b>Please correct the following error(s):</b>
          <ul>
            <li v-for="error in errors" :key="error.key"> {{error}}</li>
          </ul>
        </p>
        <p>
          <label for="name">Name</label>
          <input v-model="name" type="text" name="name">
        </p>
        <p>
          <label for="age">Age</label>
          <input v-model="age" type="number" name="age" min="0">
        </p>
        <p>
          <label for="color">Favorite Color </label>
          <select v-model="color" name="color">
            <option value="blue"> blue</option>
            <option value="red"> red</option>
            <option value="green"> green</option>
          </select>
        </p>
        <p>
          <input type="submit" value="Submit">
        </p>
      </form>
   </div>
</template>

<script>
export default {
    data: ()=>{
      return {
        errors: [],
        name: null,
        age: null,
        color: null
      }
    },
    methods: {
        checkForm (e) {
            if (this.name && this.age) {
                return true
            }

            this.errors = []

            if (!this.name) {
                this.errors.push("Name required.")
            }
            if (!this.age) {
                this.errors.push("Age required.")
            }

            e.preventDefault()
        }
    }
}
</script>

<style scoped>
</style>