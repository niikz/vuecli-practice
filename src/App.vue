<template>
  <h1>Memo APP</h1>
  <ul v-for="memo in memos" :key="memo.id">
    <li>{{ memo.text.split('\n')[0] }}</li>
  </ul>
</template>

<script>
export default {
  name: 'App',
  data () {
    return {
      memos: [],
      memo: {
        id: '',
        text: ''
      }
    }
  },
  created () {
    this.memos = [
      { id: 1, text: 'go to sleep\ntake a nap' },
      { id: 2, text: 'eat lunch' },
      { id: 3, text: 'play game' }
    ]
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
