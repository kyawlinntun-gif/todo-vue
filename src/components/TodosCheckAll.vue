<template>
  <div><label><input type="checkbox" :checked="checkedAllTodos" @change="checkedAllTodosItems">Check All</label></div>
</template>

<script>
export default {
  name: "TodosCheckAll",
  props: {
    checkedAllTodos: {
      type: Boolean,
      required: true
    }
  },
  methods: {
    checkedAllTodosItems() {
      eventBus.$emit('checkedAll');
    }
  }
}
</script>

<style scoped>

</style>
