<template>
  <button v-if="anyCompleted" @click="clearCompleted">Clear Completed</button>
</template>

<script>
export default {
  name: "ClearCompleted",
  props: {
    anyCompleted: {
      type: Boolean,
      required: true
    }
  },
  methods: {
    clearCompleted() {
      eventBus.$emit('clearCompleted');
    }
  }
}
</script>

<style scoped>

</style>
