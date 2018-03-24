<template>
  <div id="app">
    <h1 v-html="title"></h1>
    <input v-model="newItem" v-on:keyup.enter="addNew">
    <ul>
      <li v-for="(item,index) in items" :key="index" v-bind:class="{finished: item.isFinished}"
        v-on:click="toggleFinish(item)">
        {{item.label}}
      </li>
    </ul>
    <p>chlid tells.me: {{ childWords }}</p>
    <hello msgFromFather='hi'
    v-on:child-tell-me-something='listenToMyBoy'></hello>
  </div>
</template>

<script>
import Store from './store.js'
import Hello from './components/Hello'
export default {
  data: function () {
    return {
      title: 'this is a todo list',
      items: Store.fetch(),
      newItem: '',
      childWords: ''
    }
  },
  components: { Hello },
  watch: {
      items: {
        handler: function (item) {
          Store.save(item)
        },
        deep: true
      }
  },
  methods: {
    toggleFinish: function (item) {
      item.isFinished = !item.isFinished
    },
    addNew: function () {
      this.items.push({
        label: this.newItem,
        isFinished: false
      })
      this.newItem = ''
    },
    listenToMyBoy: function (msg) {
      this.childWords = msg
    }
  }
}
</script>

<style>
.finished{
  text-decoration: underline;
}
#app {
  font-family: 'Avenir', Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
</style>
