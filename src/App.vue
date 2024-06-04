<script setup>
import { ref } from 'vue'

let todoList = ref([])
let todo = ref(null)
// ADDING TO DO
const addTodo = () => {
  if (validateTodo(todo.value)) {
    let id = todoList.value.length
    todoList.value.push({
      id: ++id,
      todo: todo.value,
      is_done: false
    })
    todo.value = null
  }
}
// SELECT TO DO
let selectedTodo = ref(null)
const selectTodo = (row) => {
  selectedTodo.value = row
  todo.value = row.todo
}
// UPDATING TODO
const updateTodo = () => {
  if (validateTodo(todo.value)) {
    let index = todoList.value.findIndex((t) => t.id === selectedTodo.value.id)
    index !== -1 && (todoList.value[index].todo = todo.value)
    todo.value = selectedTodo.value = null
  }
}
// MARKING AS DONE TO DO
const markAsDone = (row) => {
  if (confirm(`Are you sure want to mark ${row.todo} as done?`)) {
    let index = todoList.value.findIndex((t) => t.id === row.id)
    index !== -1 && (todoList.value[index].is_done = true)
  }
}
// DELETING TO DO
const deleteTodo = (row) => {
  if (confirm(`Are you sure want to delete ${row.todo}?`)) {
    let index = todoList.value.findIndex((t) => t.id === row.id)
    index !== -1 && todoList.value.splice(index, 1)
  }
}
//validation if user have existed
const validateTodo = (new_todo) => {
  let index = todoList.value.findIndex((t) => t.todo === new_todo)
  if (index !== -1) {
    alert('Todo already exist!')
    return false
  } else {
    return true
  }
}
</script>
<template>
  <div class="d-flex justify-content-center container">
    <div class="w-75">
      <!-- SELECT UPDATE OR ADDING IN TO DO -->
      <form @submit.prevent="!selectedTodo ? addTodo() : updateTodo()">
        <div class="mb-3 mt-5">
          <label for="todo">What's your todo?</label>
          <div class="d-flex">
            <input
              v-model="todo"
              type="text"
              class="form-control"
              id="todo"
              placeholder="eg. Create todo application"
            />
            <!-- CONDITIONAL RENDERING -->
            <button v-if="!selectedTodo" class="btn btn-primary ms-3">
              <i class="bi bi-plus-circle-fill"></i>
            </button>
            <button v-else class="btn btn-primary ms-3"><i class="bi bi-pencil-square"></i></button>
          </div>
        </div>
      </form>
      <!-- LOOP IN SELECTING ID IN TODO -->
      <div
        @click="selectTodo(row)"
        v-for="row in todoList"
        :key="row.id"
        class="toast show mt-2 w-100"
      >
        <div class="toast-header">
          <strong class="me-auto">{{ row.todo }}</strong>
          <!-- BTN IN MARK AS DONE TO DO  -->
          <button
            :class="`btn btn-${row.is_done ? 'success' : 'outline-secondary'} btn-sm`"
            @click="markAsDone(row)"
          >
            <i class="bi bi-check-lg"></i>
          </button>
          <!-- BTN IN DELETING TO DO -->
          <button class="btn btn-danger btn-sm ms-1" @click="deleteTodo(row)">
            <i class="bi bi-trash-fill"></i>
          </button>
        </div>
      </div>
    </div>
  </div>
</template>

<style scoped></style>
