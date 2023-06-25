<script setup>
// IMPORT FIELD
import { ref, watch, onMounted, computed } from 'vue';
import { getItem, setItem } from './utils/localStorageUtils';

import HeaderComp from './components/HeaderComp.vue';
import InputTodo from './components/InputTodo.vue';
import TodoList from './components/TodoList.vue';
import FooterComp from './components/FooterComp.vue';

// VARIABLE
const todos = ref([]);
const activeOption = ref('All');

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
      <div class="min-h-screen max-w-xl mx-auto pt-[20vh] px-[10px]">
         <HeaderComp />
         <InputTodo @onEnter="addTodo" />
         <TodoList :filteredTodos="filteredTodos" @onDelete="deleteTodo" />
         <!-- <ul class="rounded-t-lg overflow-hidden">
            <VueDraggableNext
               v-model="filteredTodos"
               @start="drag = true"
               @end="drag = false"
            >
               <li
                  v-for="todo in filteredTodos"
                  class="card flex items-cente justify-between border-b-slate-50 cursor-pointer"
               >
                  <div class="flex items-center gap-5">
                     <div
                        class="circle relative flex items-center justify-center"
                        :class="
                           todo.isDone
                              ? 'bg-gradient-to-br from-cyan-500 via-80%  via-purple-500  to-purple-700 border-none'
                              : 'hover:border-t-cyan-500 hover:border-l-cyan-500 hover:border-b-purple-700 hover:border-r-purple-700'
                        "
                     >
                        <input
                           type="checkbox"
                           class="absolute cursor-pointer w-full h-full appearance-none"
                           v-model="todo.isDone"
                        />
                        <img
                           v-show="todo.isDone"
                           src="images/icon-check.svg"
                           alt="check icon"
                        />
                     </div>
                     <p
                        class="header"
                        :class="{ 'line-through opacity-75': todo.isDone }"
                     >
                        {{ todo.name }}
                     </p>
                  </div>
                  <button @click="deleteTodo">
                     <svg
                        xmlns="http://www.w3.org/2000/svg"
                        width="18"
                        height="18"
                     >
                        <path
                           fill="#494C6B"
                           :class="{ 'fill-slate-300': isDark }"
                           fill-rule="evenodd"
                           d="M16.97 0l.708.707L9.546 8.84l8.132 8.132-.707.707-8.132-8.132-8.132 8.132L0 16.97l8.132-8.132L0 .707.707 0 8.84 8.132 16.971 0z"
                        />
                     </svg>
                  </button>
               </li>
            </VueDraggableNext>
         </ul> -->
         <FooterComp
            :filteredTodos="filteredTodos"
            :activeOption="activeOption"
            @onSelect="selectFilterOption"
            @onClear="clearCompletedTodo"
         />
      </div>
   </div>
</template>
