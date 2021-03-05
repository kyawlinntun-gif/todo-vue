<template>
  <div>
    <input type="text" class="todo-input" placeholder="What needs to be done" v-model="newTodo" @keyup.enter="addTodo">

    <transition-group name="fade" enter-active-class="animated fadeInUp" leave-active-class="animated fadeOutDown">
      <div v-for="(todo, index) in filterTodos" v-bind:key="todo.id">
        <todo-item :todo="todo" :index="index" :checkedAll="anyRemaining"></todo-item>
      </div>
    </transition-group>

    <div class="extra-container">
      <TodosCheckAll :checkedAllTodos="anyRemaining"></TodosCheckAll>
      <TodoItemsRemaining :items-remaining="remaining"></TodoItemsRemaining>
    </div>

    <div class="extra-container">
      <TodoItemsFilters></TodoItemsFilters>
      <div>
        <transition name="fade">
          <ClearCompleted :any-completed="anyCompleted"></ClearCompleted>
        </transition>
      </div>
    </div>

  </div>
</template>

<script>
import TodoItem from "./TodoItem";
import TodoItemsRemaining from "./TodoItemsRemaining";
import TodosCheckAll from "./TodosCheckAll";
import TodoItemsFilters from "./TodoItemsFilters";
import ClearCompleted from "./ClearCompleted";
export default {
  name: "TodoList",
  components: {
    TodoItem, TodoItemsRemaining, TodosCheckAll, TodoItemsFilters, ClearCompleted
  },
  created() {
    eventBus.$on('removeTodo', (index) => this.removeTodo(index));
    eventBus.$on('finishedDone', (data) => this.finishedDone(data));
    eventBus.$on('checkedAll', () => this.checkAllTodos());
    eventBus.$on('changedFilters', (filter) => this.filter = filter);
    eventBus.$on('clearCompleted', () => this.clearCompleted());
  },
  data() {
    return {
      newTodo: '',
      idForTodo: 3,
      beforeEditCache: '',
      filter: 'all',
      todos: [
        {
          'id': 1,
          'title': 'Finished vue screencast',
          'completed': false,
          'editing': false
        },
        {
          'id': 2,
          'title': 'Take over world',
          'completed': false,
          'editing': false
        }
      ]
    }
  },
  computed: {
    remaining() {
      return this.todos.filter(todo => !todo.completed).length;
    },
    anyRemaining() {
      // return this.todos.filter(todo => !todo.completed).length == 0;
      return this.remaining == 0;
    },
    filterTodos() {
      if(this.filter == 'all') {
        return this.todos;
      } else if(this.filter == 'active')
      {
        return this.todos.filter(todo => !todo.completed);
      } else if(this.filter == 'completed')
      {
        return this.todos.filter(todo => todo.completed);
      }
      return this.todos;
    },
    anyCompleted() {
      return this.todos.filter(todo => todo.completed).length > 0;
    }
  },
  methods: {
    addTodo() {
      if(this.newTodo.trim().length == 0)
      {
        return;
      }
      this.todos.push({
        'id': this.idForTodo,
        'title': this.newTodo,
        'completed': false,
        'editing': false
      });

      this.newTodo = '';
      this.idForTodo++;
    },
    removeTodo(index) {
      this.todos.splice(index,1);
    },
    finishedDone(data)
    {
      this.todos.splice(data.index, 1, data.todo);
    },
    checkAllTodos()
    {
      this.todos.forEach(todo => todo.completed = event.target.checked);
    },
    clearCompleted()
    {
      this.todos = this.todos.filter(todo => !todo.completed);
    }
  }
}
</script>

<style lang="scss">
@import url("https://cdnjs.cloudflare.com/ajax/libs/animate.css/3.5.2/animate.min.css");

.todo-input {
  width: 100%;
  padding: 10px 18px;
  font-size: 18px;
  margin-bottom: 16px;

  &:focus {
    outline: 0;
  }
}
.todo-item {
  margin-bottom: 12px;
  display: flex;
  align-items: center;
  justify-content: space-between;
  animation-duration: 0.3s;
}
.remove-item {
  cursor: pointer;
  margin-left: 14px;
  &:hover {
    color: black;
  }
}
.todo-item-left { //later
  display: flex;
  align-items: center;
}
.todo-item-label {
  padding: 10px;
  border: 1px solid white;
  marign-left: 12px;
}
.todo-item-edit {
  font-size: 24px;
  color: #2c3e50;
  margin-left: 12px;
  width: 100%;
  padding: 10px;
  border: 1px solid #ccc; //override default
  font-family: 'Avenir', Helvetica, Arial, sans-serif;

  &:focus {
    outline: none;
  }
}
.completed {
  text-decoration: line-through;
  color: grey;
}
.extra-container {
  display: flex;
  align-items: center;
  justify-content: space-between;
  font-size: 16px;
  border-top: 1px solid lightgrey;
  padding-top: 14px;
  margin-bottom: 14px;
}
button {
  font-size: 14px;
  background-color: white;
  appearance: none;

  &:hover {
    background: lightgreen;
  }

  &:focus {
    outline: none;
  }
}

// CSS Transitions
.fade-enter-active, .fade-leave-active {
  transition: opacity 0.2s;
}
.fade-enter, .fade-leave-to {
  opacity: 0;
}
</style>




















