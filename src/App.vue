<template>
  <h1>Memo APP</h1>
  <ul>
    <memo-list
      v-for="memo in memos"
      :key="memo.id"
      :memo="memo"
    ></memo-list>
  </ul>
  <button type="button" @click.prevent="newMemo">新規メモ</button>
  <form>
    <textarea type="text" ref="input" v-model="editText"></textarea>
    <button type="button" @click="addMemo">追加</button>
  </form>
</template>

<script>
import MemoList from '@/components/MemoList'

export default {
  name: 'App',
  components: { MemoList },
  data () {
    return {
      memos: [],
      memo: {
        id: '',
        text: ''
      },
      editText: ''
    }
  },
  created () {
    this.memos = [
      { id: 1, text: 'go to sleep\ntake a nap' },
      { id: 2, text: 'eat lunch' },
      { id: 3, text: 'play game' }
    ]
  },
  methods: {
    newMemo () {
      this.$refs.input.focus()
      this.editText = 'New Memo'
    },
    addMemo () {
      const memo = {
        id: Number(Date.now()).toString(16),
        text: this.editText
      }
      if (memo.text.length > 0) {
        this.memos.push(memo)
      }
      this.editText = ''
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
