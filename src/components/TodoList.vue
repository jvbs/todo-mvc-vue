<template>
  <div class="todo">
    <input
      v-model="newTodo"
      @keyup.enter="addTodo"
      type="text"
      class="todo-input"
      placeholder="What needs to be done?">
    <transition-group
      name="fade"
      enter-active-class="animated fadeInUp"
      leave-active-class="animated fadeOutDown">
      <div
        v-for="(todo, index) in todosFiltered" :key="todo.id"
        class="todo-item">
        <div class="todo-item-left">
          <input v-model="todo.completed" type="checkbox">
          <div
            v-if="!todo.editMode"
            class="todo-item-label"
            :class="{ completed : todo.completed }"
            @dblclick="editTodo(todo)">{{ todo.title }}</div>
          <input
            v-else
            v-model="todo.title"
            v-focus
            @blur="doneEdit(todo)"
            @keyup.enter="doneEdit(todo)"
            @keyup.esc="cancelEdit(todo)"
            class="todo-item-edit"
            type="text"
            >
        </div>
        <div class="remove-item" @click="removeTodo(index)">
          &times;
        </div>
      </div>
    </transition-group>
    <div class="extra-container">
      <div>
        <label>
          <input
            type="checkbox"
            @change="checkAllTodos"
            :checked="!anyRemaining"> Marcar todos
        </label>
      </div>
      <div>
        {{ remaining }} itens
      </div>
    </div>

    <div class="extra-container">
      <div>
        <button
          :class="{ active: filter == 'all'}"
          @click="filter = 'all'">Todos</button>
        <button
          :class="{ active: filter == 'active'}"
          @click="filter = 'active'">Ativas</button>
        <button
          :class="{ active: filter == 'completed'}"
          @click="filter = 'completed'">Completas</button>
      </div>

      <div>
        <transition name="fase">
          <button v-if="showClearCompletedButton" @click="clearCompleted">Excluir finalizadas</button>
        </transition>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  name: 'todo-list',
  data () {
    return {
      newTodo: '',
      idForTodo: 4,
      beforeEditCache: '',
      filter: 'all',
      todos: [
        {
          'id': 1,
          'title': 'Terminar ToDo APP',
          'completed': false,
          'editMode': false
        },
        {
          'id': 2,
          'title': 'Criar roteiro pro video',
          'completed': false,
          'editMode': false
        },
        {
          'id': 3,
          'title': 'Publicar no youtube',
          'completed': false,
          'editMode': false
        }
      ]
    }
  },

  directives: {
    focus: {
      // directive definition
      inserted: function(el){
        el.focus()
      }
    }
  },

  computed: {
    remaining(){
      return this.todos.filter(todo => !todo.completed).length
    },

    anyRemaining(){
      return this.remaining != 0
    },

    todosFiltered(){
      if(this.filter == 'all'){
        return this.todos
      } else  if(this.filter == 'active'){
        return this.todos.filter(todo => !todo.completed)
      } else if(this.filter == 'completed'){
        return this.todos.filter(todo => todo.completed)
      }
    },

    showClearCompletedButton(){
      return this.todos.filter(todo => todo.completed).length > 0
    }
  },

  methods: {
    addTodo(){
      if(this.newTodo.trim().length == 0){
        return
      }

      this.todos.push({
        id: this.idForTodo,
        title: this.newTodo,
        completed: false
      })

      this.newTodo = ''
      this.idForTodo++
    },

    editTodo(todo){
      this.beforeEditCache = todo.title
      todo.editMode = true
    },

    cancelEdit(todo){
      todo.title = this.beforeEditCache
      todo.editMode = false
    },

    doneEdit(todo){
      if(todo.title.trim().length == ''){
        todo.title = this.beforeEditCache
      }
      todo.editMode = false
    },

    removeTodo(index){
      this.todos.splice(index, 1)
    },

    checkAllTodos(){
      this.todos.forEach((todo) => todo.completed = event.target.checked)
    },

    clearCompleted(){
      this.todos = this.todos.filter(todo => !todo.completed)
    }
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style lang="scss">
@import url("https://cdnjs.cloudflare.com/ajax/libs/animate.css/4.0.0/animate.min.css");

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
}

.remove-item {
  cursor: pointer;
  margin-left: 14px;
  &:hover {
    color: #000;
  }
}

.todo-item-left {
  display: flex;
  align-items: center;
}

.todo-item-label {
  padding: 10px;
  border: 1px solid #fff;
  margin-left: 12px;
}

.todo-item-edit {
  font-size: 24px;
  color: #2c3e50;
  margin-left: 12px;
  width: 100%;
  padding: 10px;
  border: 1px solid #ccc;
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
  border-top: 1px solid lightgray;
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

.active {
  background: lightgreen;
}

// CSS Transitions
.fade-enter-active, .fade-leave-active {
  transition: opacity .2s;
}

.fade-enter, .fade-leave-to {
  opacity: 0;
}
</style>
