<template>
  <li @click.prevent="$emit('edit-memo', memo)">{{ memoTitle }}</li>
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
