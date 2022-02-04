<template>
  <li @click.prevent="$emit('edit-memo', memo)">
    {{ memoTitle }}
  </li>
</template>

<script>
export default {
  name: 'MemoList',
  props: {
    memo: {}
  },
  emits: ['edit-memo'],
  computed: {
    memoTitle () {
      return this.memo.text.split('\n')[0]
    }
  }
}
</script>

<style scoped>
li {
  text-decoration: underline;
  margin: 5px 0;
  padding-bottom: 2px;
  cursor: pointer;
}
li:hover {
  text-decoration: none;
}
</style>
