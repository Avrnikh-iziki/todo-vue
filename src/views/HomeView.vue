<script setup lang="ts">
import { Icon } from '@iconify/vue'
import { uid } from 'uid' 
import { ref, computed } from 'vue'
import TodoCreator from '../components/TodoCreator.vue'
import TodoItem from '../components/TodoItem.vue'
type todoType = {
  id: string
  isCompleted: boolean
  isEditing: boolean | null 
  todo: string
}
const todoList = ref<Array<todoType>>([])

const todosCompleted = computed(() => {
  return todoList.value.every((todo: todoType) => todo.isCompleted)
})
const fetchTodoList = () => {
  const savedTodoList = JSON.parse(localStorage.getItem('todoList') || "null")
  if (savedTodoList) {
    todoList.value = savedTodoList
  }
}

fetchTodoList()

const setTodoListLocalStorage = () => {
  localStorage.setItem('todoList', JSON.stringify(todoList.value))
}

const createTodo = (todo: string) => {
  todoList.value.push({
    id: uid(),
    todo,
    isCompleted: false,
    isEditing: null
  })
  setTodoListLocalStorage()
}

const toggleEditTodo = (todoPos: number) => {
  todoList.value[todoPos].isEditing = !todoList.value[todoPos].isEditing
  setTodoListLocalStorage()
}

const updateTodo = (todoVal:string, todoPos: number) => {
  todoList.value[todoPos].todo = todoVal
  setTodoListLocalStorage()
}

const toggleTodoComplete = (todoPos: number) => {
  todoList.value[todoPos].isCompleted = !todoList.value[todoPos].isCompleted
  setTodoListLocalStorage()
}

const deleteTodo = (todo: todoType) => {
  todoList.value = todoList.value.filter((todoFilter) => todoFilter.id !== todo.id)
  setTodoListLocalStorage()
}
</script>

<template>
  <main>
    <h4>Create Todo</h4>
    <TodoCreator @create-todo="createTodo">
      <template #button-content>Create</template>
    </TodoCreator>
    <ul class="todo-list" v-if="todoList.length > 0">
      <TodoItem
        v-for="(todo, index) in todoList"
        v-bind:key="index"
        :todo="todo"
        :index="index"
        @edit-todo="toggleEditTodo"
        @update-todo="updateTodo"
        @toggle-complete="toggleTodoComplete"
        @delete-todo="deleteTodo"
      />
    </ul>
    <p v-else class="todos-msg">
      <Icon icon="noto-v1:sad-but-relieved-face" />
      <span>You have no todo's to complete! Add one!</span>
    </p>
    <p v-if="todosCompleted && todoList.length > 0" class="todos-msg">
      <Icon icon="noto-v1:party-popper" />
      <span>You have completed all your todos!</span>
    </p>
  </main>
</template>

<style scoped lang="scss">
main {
  display: flex;
  flex-direction: column;
  max-width: 500px;
  width: 100%;
  margin: 0 auto;
  padding: 40px 16px;

  h4 {
    margin-bottom: 16px;
    text-align: center;
  }

  .todo-list {
    display: flex;
    flex-direction: column;
    list-style: none;
    margin-top: 24px;
    gap: 20px;
  }

  .todos-msg {
    display: flex;
    align-items: center;
    justify-content: center;
    gap: 8px;
    margin-top: 24px;
  }
}
</style>
