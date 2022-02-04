<template>
  <li>{{ memoTitle }}</li>
</template>

<script>
export default {
  name: 'MemoList',
  props: {
    memo: {}
  },
  computed: {
    memoTitle () {
      return this.memo.text.split('\n')[0]
    }
  }
}
</script>
