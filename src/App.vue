<template>
  <div id="app">
    <SearchAmmo/>
    <tabs />
    <ToTop/>
    <Popup>
      <h1>hi</h1>
      <p>hi</p>
    </Popup>
  </div>
</template>

<script>
import tabs from "./components/Tabs.vue";
import SearchAmmo from "./components/SearchAmmo.vue";
import ToTop from "./components/ToTop.vue";
import Popup from "./components/Popup.vue";

export default {
  name: "App",
  components: {
    SearchAmmo,
    tabs,
    ToTop,
    Popup
  },
};
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin: 0;
  padding: 0;
}
.container {
  max-width: 920px;
  margin: 30px auto 0;
}
body {
  padding: 0;
  margin: 0;
}
</style>
