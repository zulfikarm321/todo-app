<script setup>
import { ref, defineEmits } from 'vue';

const emit = defineEmits(['onEnter']);
const inputTodo = ref('');

const add = () => {
   emit('onEnter', inputTodo.value);
   inputTodo.value = '';
};
</script>

<template>
   <div class="card flex items-center my-5 gap-5 rounded-lg">
      <div class="circle"></div>
      <input
         class="outline-none bg-transparent dark:placeholder:text-slate-300 w-full"
         type="text"
         placeholder="Create a new todo..."
         v-model="inputTodo"
         @keyup.enter="add"
      />
   </div>
</template>
