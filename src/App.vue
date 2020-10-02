<template>
  <div>
     <header>
        <span v-on:click="ChangePage('Home')">Home</span>
        <span v-on:click="ChangePage('Stats')">Stats</span>
    </header>
    <Home v-if="page === 'Home'" />
    <Stats v-else-if="page === 'Stats'"/>
  </div>
</template>

<script>
import Home from './components/Home.vue'
import Stats from './components/Stats.vue'

export default {
  name: 'App',
  components: {
    Home,
    Stats
  },
  data() {
    return {
      page: 'Home'
    }
  },
  methods: {
    ChangePage(clickedPage) {
      this.page = clickedPage;
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
}

header {
  border: 1px solid #e7e7e7;
  background-color: #f3f3f3;
  padding: 5px;
}
header span {
  background-color: #f3f3f3;
  cursor: pointer;
  margin: 5px;
}

header span:hover {
  color: #ffa200;
}
</style>
