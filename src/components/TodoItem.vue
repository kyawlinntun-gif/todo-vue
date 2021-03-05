<template>
  <div class="todo-item">
    <div class="todo-item">
      <div class="todo-item-left">
        <input type="checkbox" v-model="completed" @change="doneEdit">
        <div class="todo-item-label" v-if="!editing" @dblclick="editTodo" :class="{ completed : completed }">{{ title }}</div>
        <input type="text" class="todo-item-edit" v-model="title" v-else v-focus @blur="doneEdit" @keyup.enter="doneEdit">
      </div>
    </div>
    <div>
      <button @click="pluralize">Plural</button>
      <span class="remove-item" @click="removeTodo">
        &times;
      </span>
    </div>
  </div>
</template>

<script>
export default {
  name: "TodoItem",
  props: {
    todo: {
      required: true,
      type: Object
    },
    index: {
      required: true,
      type: Number
    },
    checkedAll: {
      required: true,
      type: Boolean
    }
  },
  data() {
    return {
      'id': this.todo.id,
      'title': this.todo.title,
      'completed': this.todo.completed,
      'editing': this.todo.editing,
      'beforeEditCache': ''
    }
  },
  watch: {
    checkedAll() {
      this.completed = this.checkedAll ? true : this.todo.completed;
    }
  },
  created() {
    eventBus.$on('pluralize', this.handlePluralize);
  },
  beforeDestroy() {
    eventBus.$off('pluralize', this.handlePluralize);
  },
  methods: {
    removeTodo() {
      eventBus.$emit('removeTodo', this.index);
    },
    editTodo(){
      this.beforeEditCache = this.title;
      this.editing = true;
    },
    doneEdit(index){
      if(this.title.trim().length == 0)
      {
        this.title = this.beforeEditCache;
      }
      this.editing = false;

      eventBus.$emit('finishedDone', {
        'index': this.index,
        'todo': {
          'id': this.id,
          'title': this.title,
          'completed': this.completed,
          'editing': this.editing,
        }
      });
    },
    pluralize() {
      eventBus.$emit('pluralize');
    },
    handlePluralize() {
      this.title = this.title + 's';
      eventBus.$emit('finishedDone', {
        'index': this.index,
        'todo': {
          'id': this.id,
          'title': this.title,
          'completed': this.completed,
          'editing': this.editing,
        }
      });
    }
  },
  directives: {
    focus: {
      //  directive definition
      inserted: function (el) {
        el.focus();
      }
    }
  }
}
</script>

<style scoped>

</style>
