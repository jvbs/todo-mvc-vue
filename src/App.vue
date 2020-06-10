<template>
  <div id="app" class="container">
    <!-- <img src="./assets/logo.png" class="logo"> -->
    <h1 class="text-center">Todo List App</h1>
    <todo-list/>
  </div>
</template>

<script>
import HelloWorld from './components/HelloWorld'
import TodoList from './components/TodoList'

export default {
  name: 'App',
  components: {
    HelloWorld,
    TodoList
  }
}
</script>

<style>
* {
  box-sizing: border-box;
}

.container {
  max-width: 600px;
  margin: 0 auto;
}

.text-center {
  text-align: center;
}

.logo {
  display: block;
  margin: 20px auto;
  height: 75px;
}

#app {
  font-family: 'Avenir', Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  /* text-align: center; */
  color: #2c3e50;
  margin-top: 60px;
  font-size: 24px;
}
</style>
