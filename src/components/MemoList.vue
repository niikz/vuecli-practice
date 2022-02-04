<template>
  <li>{{ memo.text.split('\n')[0] }}</li>
</template>

<script>
export default {
  name: 'MemoList',
  props: {
    memo: {}
  }
}
</script>
