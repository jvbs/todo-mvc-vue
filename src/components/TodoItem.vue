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
    <div class="remove-item" @click="removeTodo(index)">
      &times;
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
  watch: {
    checkAll(){
      this.completed = this.checkAll ? true : this.todo.completed
    }
  },
  methods: {
    removeTodo(index){
      this.$emit('removedTodo', index)
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
      this.$emit('finishedEditTodo', {
        'index': this.index,
        'todo': {
          'id': this.id,
          'title': this.title,
          'completed': this.completed,
          'editMode': this.editMode
        }
      })
    },

  }
}
</script>

<style>

</style>
