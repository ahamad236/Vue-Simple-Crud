<script setup>
  import { ref, watch, onMounted } from "vue"

  const todos = ref([])
  const text = ref("")

  function addToDo(){
    if(text.value.trim() === ""){
      return
    }
    debugger
    todos.value.unshift({
      todo: text.value,
      checked: false
    })

    text.value = ""
  }

  function deleteToDo(todo){
    todos.value = todos.value.filter(x => x != todo)
  }

  watch( 
    todos, 
    (newToDoValue) => {
      debugger
      localStorage.setItem("todos", JSON.stringify(newToDoValue))
    },
    {deep: true}
  )

  onMounted( () => {
    debugger
    todos.value = JSON.parse(localStorage.getItem("todos")) || []
  })
</script>

<template>
  <section class="todo-inputs">
    <form @submit.prevent="addToDo">
      <h2>Enter the Todo's you want to Add</h2>
      <input v-model="text" />
      <button>AddtoToDoTask</button>
    </form>
  </section>

  <section class="todo-lists">
    <div v-for="todo in todos" :key=todo.todo>
      <input type="checkbox" v-model="todo.checked"/>
      <input v-model="todo.todo"/>
      <button @click="deleteToDo(todo)">Delete</button>
    </div>
  </section>
</template>

<style scoped>
  .todo-lists{
    margin-top: 30px;
  }
</style>