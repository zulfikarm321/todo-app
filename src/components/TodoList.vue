<script setup>
import { defineProps, defineEmits, ref, watch } from 'vue';
import { useDark } from '@vueuse/core';
import { VueDraggableNext as draggable } from 'vue-draggable-next';

const emit = defineEmits(['onDelete']);
const props = defineProps(['filteredTodos']);
const isDark = useDark();
const draggableTodos = ref(props.filteredTodos);

watch(
   () => props.filteredTodos,
   (newValue) => {
      draggableTodos.value = newValue;
   },
   { deep: true }
);
</script>
<template>
   <ul class="rounded-t-lg overflow-hidden shadow-lg">
      <draggable v-model="draggableTodos">
         <li
            v-for="todo in draggableTodos"
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
                     src="/images/icon-check.svg"
                     alt="check icon"
                  />
               </div>
               <p :class="{ 'line-through opacity-75': todo.isDone }">
                  {{ todo.name }}
               </p>
            </div>
            <button @click="emit('onDelete', todo.name)">
               <svg xmlns="http://www.w3.org/2000/svg" width="18" height="18">
                  <path
                     fill="#494C6B"
                     :class="{ 'fill-slate-300': isDark }"
                     fill-rule="evenodd"
                     d="M16.97 0l.708.707L9.546 8.84l8.132 8.132-.707.707-8.132-8.132-8.132 8.132L0 16.97l8.132-8.132L0 .707.707 0 8.84 8.132 16.971 0z"
                  />
               </svg>
            </button></li
      ></draggable>
   </ul>
</template>
