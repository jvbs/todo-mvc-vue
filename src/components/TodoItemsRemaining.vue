<template>
  <div>{{ remaining }} itens</div>
</template>

<script>
export default {
  name: 'todo-items-remaining',
  props: {
    remaining: {
      type: Number,
      required: true
    }
  }
}
</script>

<style>

</style>
