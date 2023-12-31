<script setup lang="ts">
import type { PropType } from 'vue'
import { Icon } from '@iconify/vue'
type todoType = {
  id: string
  isCompleted: boolean
  isEditing: boolean | null
  todo: string
}

defineProps({
  todo: {
    type: Object as PropType<todoType>,
    default: () => {}
  },
  index: {
    type: Number,
    default: 0
  }
})
defineEmits(['edit-todo', 'update-todo', 'toggle-complete', 'delete-todo'])
</script>

<template>
  <li>
    <input type="checkbox" :checked="todo.isCompleted" @input="$emit('toggle-complete', index)" />
    <div class="todo">
      <input
        v-if="todo.isEditing"
        type="text"
        :value="todo.todo"
        @input="$emit('update-todo', ($event.target as HTMLInputElement).value, index)"
      />
      <span
        v-else
        :class="{
          'completed-todo': todo.isCompleted
        }"
      >
        {{ todo.todo }}
      </span>
    </div>
    <div class="todo-actions">
      <Icon
        v-if="todo.isEditing"
        icon="ph:check-circle"
        class="icon check-icon"
        color="41b080"
        width="22"
        @click="$emit('edit-todo', index)"
      />
      <Icon
        v-else
        icon="ph:pencil-fill"
        class="icon edit-icon"
        color="41b080"
        width="22"
        @click="$emit('edit-todo', index)"
      />
      <Icon
        icon="ph:trash"
        class="icon trash-icon"
        color="f95e5e"
        width="22"
        @click="$emit('delete-todo', todo)"
      />
    </div>
  </li>
</template>

<style lang="scss" scoped>
li {
  display: flex;
  align-items: center;
  gap: 10px;
  margin-left: -40px;
  padding: 10px 10px;
  border-radius: 5px;
  background-color: #f1f1f1;
  box-shadow:
    0 20px 25px -5px rgb(0 0 0 / 0.1),
    0 8px 10px -6px rgb(0 0 0 / 0.1);

  &:hover {
    .todo-actions {
      opacity: 1;
    }
  }

  input[type='checkbox'] { 
    appearance: none;
    width: 20px;
    height: 20px;
    background-color: #fff;
    border-radius: 50%;
    box-shadow:
      0 4px 6px -1px rgb(0 0 0 / 0.1),
      0 2px 4px -2px rgb(0 0 0 / 0.1); 

    &:checked {
      background-color: #41b080;
    }
  }

  .todo {
    flex: 1;
    color: black;

    .completed-todo {
      text-decoration: line-through;
    }

    input[type='text'] {
      width: 100%;
      padding: 2px 6px;
      border: 2px solid #41b080;
    }
  }

  .todo-actions {
    display: flex;
    gap: 6px;
    opacity: 0;
    transition: 150ms ease-in-out;
    .icon {
      cursor: pointer;
    }
  }
}
</style>
