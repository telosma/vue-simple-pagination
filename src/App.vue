<template>
  <div id="app">
    <img src="./assets/logo.png">
    <h1>{{ msg }}</h1>
    <h3>Vue Simple Pagination</h3>
    <div class="demo-component">
      <pagination
        :current-page="currentPage"
        :total-count="22"
        :per-page="2"
        v-model="currentPage"
        @change="handleChangePagination($event)"
      >
      </pagination>
    </div>
  </div>
</template>

<script>
import Pagination from './components/Pagination'

export default {
  name: 'app',
  components: {
    Pagination
  },

  data () {
    return {
      msg: 'Welcome to Telosma Pagination App',
      currentPage: 1,
    }
  },

  methods: {
    handleChangePagination (event) {
      this.currentPage = event
      console.log(`Go to page ${event}`)
      // Load new data
    }
  }
}
</script>

<style lang="scss">
#app {
  font-family: 'Avenir', Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}

h1, h2 {
  font-weight: normal;
}

ul {
  list-style-type: none;
  padding: 0;
}

li {
  display: inline-block;
  margin: 0 10px;
}

a {
  color: #42b983;
}
</style>
