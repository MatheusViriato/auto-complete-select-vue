<template>
<div>
  <span><b>#Repository name:</b> {{ item.name }} </span>
  <span><b>#Language:</b> {{ item.language || 'Unknown' }}</span>
</div>
</template>

<script>
export default {
  props: {
    item: { required: true }
  }
}
</script>

