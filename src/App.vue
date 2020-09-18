<template>
  <div>
    <h1>Github API</h1>
    <div>
      <input v-model="name" type="text" />
      <button @click="newName = name">Click Here</button>
    </div>
    <div>
      <ul>
        <li v-for="repo in repos" :key="repo.id">{{ repo.name }}</li>
      </ul>
    </div>
  </div>
</template>

<script>
  import { reactive, ref, toRefs, watch } from 'vue'
  export default {
    name: 'App',
    setup() {
      const name = ref(null)
      const newName = ref(null)
      const state = reactive({ repos: [] })
      watch(async () => {
        if (newName.value) {
          let res = await fetch(
            `https://api.github.com/users/${newName.value}/repos`
          )
          let data = await res.json()
          state.repos = data

          name.value = ''
        }
      })
      return {
        name,
        newName,
        ...toRefs(state)
      }
    }
  }
</script>

<style>
  #app {
    font-family: Avenir, Helvetica, Arial, sans-serif;
    -webkit-font-smoothing: antialiased;
    -moz-osx-font-smoothing: grayscale;
    text-align: center;
    color: #2c3e50;
    margin-top: 60px;
  }
</style>
