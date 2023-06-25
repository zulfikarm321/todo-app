<script setup>
// IMPORT FIELD
import { ref, watch, onMounted, computed } from 'vue';
import { getItem, setItem } from './utils/localStorageUtils';
import { data } from './utils/InitialData';

import HeaderComp from './components/HeaderComp.vue';
import InputTodo from './components/InputTodo.vue';
import TodoList from './components/TodoList.vue';
import FooterComp from './components/FooterComp.vue';

// VARIABLE
const todos = ref([]);
const activeOption = ref([]);

// COMPUTED
const filteredTodos = computed(() => {
   switch (activeOption.value) {
      case 'Active':
         return todos.value.filter((todo) => !todo.isDone);
      case 'Completed':
         return todos.value.filter((todo) => todo.isDone);
      default:
         return todos.value;
   }
});

// METHODS
const addTodo = (value) => {
   todos.value.push({
      name: value,
      isDone: false
   });
};

const deleteTodo = (name) => {
   todos.value = todos.value.filter((todo) => todo.name !== name);
};

const clearCompletedTodo = () => {
   todos.value = todos.value.filter((todo) => !todo.isDone);
};

const selectFilterOption = (value) => {
   activeOption.value = value;
   setItem('activeOption', activeOption.value);
};

watch(
   todos,
   (newTodos) => {
      setItem('todos', newTodos);
   },
   { deep: true }
);

onMounted(() => {
   const todosLocalStorage = getItem('todos');
   if (todosLocalStorage) {
      todos.value = todosLocalStorage;
   } else {
      todos.value = data;
   }

   const activeOptionStorage = getItem('activeOption');
   if (activeOptionStorage) {
      activeOption.value = activeOptionStorage;
   }
});
</script>

<template>
   <div>
      <div class="bg-header absolute top-0 left-0 -z-10"></div>
      <div class="min-h-screen max-w-xl mx-auto py-[10vh] px-[10px]">
         <HeaderComp />
         <InputTodo @onEnter="addTodo" />
         <TodoList :filteredTodos="filteredTodos" @onDelete="deleteTodo" />
         <FooterComp
            :filteredTodos="filteredTodos"
            :activeOption="activeOption"
            @onSelect="selectFilterOption"
            @onClear="clearCompletedTodo"
         />
      </div>
   </div>
</template>
