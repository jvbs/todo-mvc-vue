<template>
  <div class="todo-item">
    <div class="todo-item-left">
      <input v-model="completed" type="checkbox" @change="doneEdit">
      <div
        v-if="!editMode"
        class="todo-item-label"
        :class="{ completed : completed }"
        @dblclick="editTodo">{{ title }}</div>
      <input
        v-else
        v-model="title"
        v-focus
        @blur="doneEdit"
        @keyup.enter="doneEdit"
        @keyup.esc="cancelEdit"
        class="todo-item-edit"
        type="text"
        >
    </div>
    <div>
      <button @click="pluralize">Plural</button>
      <span class="remove-item" @click="removeTodo(index)">
        &times;
      </span>
    </div>
  </div>
</template>

<script>
export default {
  name: 'todo-item',
  props: {
    todo: {
      type: Object,
      required: true
    },
    index: {
      type: Number,
      required: true
    },
    checkAll: {
      type: Boolean,
      required: true
    }
  },

  data(){
    return {
      'id': this.todo.id,
      'title': this.todo.title,
      'completed': this.todo.completed,
      'editMode': this.todo.editMode,
      'beforeEditCache': '',
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

  created(){
    eventBus.$on('pluralize', this.handlePluralize)
  },

  beforeDestroy(){
    eventBus.$off('pluralize', this.handlePluralize)
  },

  watch: {
    checkAll(){
      this.completed = this.checkAll ? true : this.todo.completed
    }
  },

  methods: {
    removeTodo(index){
      eventBus.$emit('removedTodo', index)
    },

    editTodo(){
      this.beforeEditCache = this.title
      this.editMode = true
    },

    cancelEdit(){
      this.title = this.beforeEditCache
      this.editMode = false
    },

    doneEdit(){
      if(this.title.trim() == ''){
        this.title = this.beforeEditCache
      }
      this.editMode = false
      eventBus.$emit('finishedEditTodo', {
        'index': this.index,
        'todo': {
          'id': this.id,
          'title': this.title,
          'completed': this.completed,
          'editMode': this.editMode
        }
      })
    },

    pluralize(){
      eventBus.$emit('pluralize')
    },

    handlePluralize(){
      this.title = this.title + 's'
      eventBus.$emit('finishedEditTodo', {
        'index': this.index,
        'todo': {
          'id': this.id,
          'title': this.title,
          'completed': this.completed,
          'editMode': this.editMode
        }
      })
    }
  }
}
</script>

<style>

</style>
