<template>
  <h1>Memo APP</h1>
  <ul v-if="memos.length">
    <memo-list
      v-for="memo in memos"
      :key="memo.id"
      :memo="memo"
      @edit-memo="editMemo(memo)"
    ></memo-list>
  </ul>
  <p v-else>メモがありません。</p>
  <button type="button" @click.prevent="newMemo">新規メモ</button>
  <form>
    <textarea type="text" ref="input" v-model="editText"></textarea>
    <button type="button" @click="addMemo"><span v-if="edit === 0">追加</span><span v-else>更新</span></button>
    <button type="button" @click="deleteMemo">削除</button>
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
      edit: 0,
      editText: ''
    }
  },
  created () {
    this.memos = [
      { id: 1, text: 'go to sleep\ntake a nap' },
      { id: 2, text: 'eat lunch' },
      { id: 3, text: 'play game' }
    ]
    this.loadMemos()
  },
  methods: {
    newMemo () {
      this.$refs.input.focus()
      this.edit = 0
      this.editText = 'New Memo'
    },
    editMemo (memo) {
      this.$refs.input.focus()
      this.edit = memo.id
      this.editText = memo.text
    },
    addMemo () {
      const memo = {
        id: this.edit === 0 ? Number(Date.now()).toString(16) : this.edit,
        text: this.editText
      }
      if (memo.text.length > 0) {
        if (this.edit === 0) {
          this.memos.push(memo)
        } else {
          const index = this.memos.findIndex(memo => memo.id === this.edit)
          this.memos.splice(index, 1, memo)
        }
      }
      this.saveMemos()
    },
    deleteMemo () {
      this.memos = this.memos.filter(memo => {
        return this.edit !== memo.id
      })
      this.resetMemoData()
    },
    saveMemos () {
      const json = JSON.stringify(this.memos)
      localStorage.setItem('memo', json)
      this.resetMemoData()
    },
    loadMemos () {
      const json = localStorage.getItem('memo')
      if (json != null) {
        this.memos = JSON.parse(json)
      }
    },
    resetMemoData () {
      this.edit = 0
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
