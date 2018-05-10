<template>
  <div id="app">
    <img src="./assets/logo.png">
    <h1>{{ msg }}</h1>
    <h3>Vue Simple Pagination</h3>
    <div class="demo-component">
      <pagination
        :current-page="currentPage"
        :total-count="totalCount"
        :per-page="perPage"
        :type="type"
        v-model="currentPage"
        @change="handleChangePagination($event)"
      >
      </pagination>
    </div>
    <div class="ui two column centered grid">
      <div class="column">
        <a class="ui teal tag label">Type</a>
        <select class="ui dropdown" v-model="type">
          <option value="df">Default</option>
          <option value="simple">Simple</option>
        </select>
      </div>
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
      perPage: 2,
      totalCount: 22,
      type: "simple"
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

.demo-component {
  padding: 2rem;
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
