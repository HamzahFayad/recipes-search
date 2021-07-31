<template>
  <div id="app">
    <h1>Search Recipe API</h1>
    <p class="note">* Only 10 requests per minute are possible.</p>
    <div class="search">
      <Search />
    </div>
  </div>
</template>

<script>
import Search from "./components/Search.vue";

export default {
  name: "App",
  components: {
    Search,
  },
};
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  color: #2c3e50;
  margin: 3.5%;
  padding: 0;
}
h1,
.note {
  text-align: center;
}
.note {
  font-size: 13px;
}
/* h1 {
  font-size: 24px;
}
h2 {
  font-size: 20px;
}
h3 {
  font-size: 18px;
}
h4 {
  font-size: 16px;
}
p {
  font-size: 14px;
} */
</style>
