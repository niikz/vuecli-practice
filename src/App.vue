<template>
  <h1>Memo APP</h1>
  <div class="contents">
    <div class="memo_list">
      <ul v-if="existMemo">
        <memo-list
          v-for="memo in memos"
          :key="memo.id"
          :memo="memo"
          @edit-memo="editMemo(memo)"
        ></memo-list>
      </ul>
      <p v-else>メモがありません。</p>
      <button class="button add" type="button" @click="newMemo">新規メモ</button>
    </div>
    <form>
      <textarea type="text" ref="input" v-model="editText"></textarea>
      <div class="button_contents">
        <button v-if="isNewMemo" class="button edit" type="button" @click="addMemo">追加</button>
        <button v-else class="button edit" type="button" @click="updateMemo">更新</button>
        <button class="button delete" type="button" @click="deleteMemo">削除</button>
      </div>
    </form>
  </div>
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
      selectedId: 0,
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
  computed: {
    existMemo () {
      return this.memos.length > 0
    },
    isNewMemo () {
      return this.selectedId === 0
    }
  },
  methods: {
    newMemo () {
      this.resetMemoData()
      this.$refs.input.focus()
      this.editText = 'New Memo'
    },
    editMemo (memo) {
      this.$refs.input.focus()
      this.selectedId = memo.id
      this.editText = memo.text
    },
    addMemo () {
      const memo = {
        id: Number(Date.now()).toString(16),
        text: this.editText
      }
      if (memo.text.length > 0) {
        this.memos.push(memo)
      }
      this.saveMemos()
    },
    updateMemo () {
      const memo = {
        id: this.selectedId,
        text: this.editText
      }
      if (memo.text.length > 0) {
        const index = this.memos.findIndex(memo => memo.id === this.selectedId)
        this.memos.splice(index, 1, memo)
      }
      this.saveMemos()
    },
    deleteMemo () {
      this.memos = this.memos.filter(memo => {
        return this.selectedId !== memo.id
      })
      this.saveMemos()
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
      this.selectedId = 0
      this.editText = ''
    }
  }
}
</script>

<style>
@import "assets/stylesheets/reset.css";

#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
}
h1 {
  text-align: center;
  margin: 0 auto 20px;
}
.contents {
  display: flex;
  flex-wrap: wrap;
  justify-content: space-between;
}
.memo_list {
  width: 25%;
}
form {
  width: 70%;
}
textarea {
  border-radius: 5px;
  min-width: 200px;
  min-height: 200px;
  padding: 7px 10px;
}
.button_contents {
  display: flex;
  flex-wrap: wrap;
  justify-content: space-between;
  width: 100%;
}

/* button */
.button {
  background-color: #fff;
  border: solid 1px #d1d1d1;
  border-radius: 5px;
  min-width: 60px;
  font-size: 18px;
  font-weight: 700;
  line-height: 1.8;
  text-decoration: none;
  margin-top: 5px;
  padding: 0 5px;
  cursor: pointer;
}
.button:hover {
  opacity: 0.8;
}
.button.add {
  background-color: #f5c23a;
  width: 100%;
}
.button.edit {
  color: #fff;
  background-color: #229bcc;
  width: 65%;
}
.button.delete {
  color: #fff;
  background-color: #ff5454;
  width: 30%;
}
</style>
