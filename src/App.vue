<template>
  <div id="app">
    <img class="logo-img" alt="Vue logo" src="./assets/logo.png" />
    <p>Path: {{ path }}</p>
    <button @click="focus">Enable Keyboard Navigation</button>
    <div class="tree-component">
      <render-component
        v-for="item in items"
        :key="item.name"
        :item="item"
        :path="`${item.name}`"
        @select="(value) => (path = value)"
      />
    </div>
  </div>
</template>

<script>
import * as items from '../public/static/node_modules.json'
import RenderComponent from '@/components/MainComponent/RenderComponent.vue'

export default {
  name: 'App',
  components: { RenderComponent },
  data: () => ({
    items,
    path: '',
  }),
  methods: {
    focus() {
      this.$el.querySelector('.wrapper').focus()
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

p {
  text-align: center;
}

button {
  display: block;
  margin: auto;
  margin-bottom: 20px;
  padding: 12px 20px;
  border: 1px solid #000;
  background: #fff;
  cursor: pointer;
  transition: background-color 300ms;
}

button:hover {
  background: #000;
  color: #fff;
}

.logo-img {
  width: 100px;
  display: block;
  margin: auto;
}

.wrapper:focus {
  outline: 1px solid black;
}
</style>
