<template>
  <div>
    <div>{{ itemsRemaining }} items left</div>
  </div>
</template>

<script>
export default {
  name: "TodoItemsRemaining",
  props: {
    itemsRemaining: {
      type: Number,
      required: true
    }
  }
}
</script>

<style scoped>

</style>
