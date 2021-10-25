<template>
  <div id="app">
    <img class="logo-img" alt="Vue logo" src="./assets/logo.png" />

    <div class="tree-component">
      <render-component
        v-for="(item, i) in items"
        :key="i"
        :item="item"
        :shown="shown"
        @unfold="unfoldFolder"
      />
    </div>
  </div>
</template>

<script>
import * as items from '../public/static/node_modules.json'
import RenderComponent from './components/MainComponent/RenderComponent.vue'

export default {
  name: 'App',
  components: { RenderComponent },
  data: () => ({
    items,
    shown: false,
  }),
  methods: {
    unfoldFolder(item) {
      console.log(item)
    },
  },
}
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  font-size: 22px;
  color: #2c3e50;
  margin-top: 20px;
}

.logo-img {
  width: 100px;
  display: block;
  margin: auto;
}
</style>
