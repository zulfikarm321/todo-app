<script setup>
import { defineProps, defineEmits } from 'vue';

const emit = defineEmits(['onSelect', 'onClear']);
const props = defineProps(['filteredTodos', 'activeOption']);

const filterOption = ['All', 'Active', 'Completed'];
</script>
<template>
   <div>
      <div
         class="card flex justify-between text-sm rounded-b-lg overflow-hidden"
      >
         <p>{{ filteredTodos.length }} Items left</p>
         <div class="hidden sm:flex items-center gap-5">
            <button
               v-for="value in filterOption"
               :key="value"
               class="cursor-pointer"
               :class="
                  value === activeOption
                     ? 'text-blue-500'
                     : '  hover:text-slate-900 dark:hover:text-white'
               "
               @click="emit('onSelect', value)"
            >
               {{ value }}
            </button>
         </div>
         <button class="cursor-pointer" @click="emit('onClear')">
            Clear Completed
         </button>
      </div>

      <div class="card flex sm:hidden justify-center text-sm mt-5 rounded-lg">
         <div class="flex items-center gap-5">
            <button
               v-for="value in filterOption"
               :key="value"
               class="cursor-pointer"
               :class="
                  value === activeOption
                     ? 'text-blue-500'
                     : '  hover:text-slate-900 dark:hover:text-white'
               "
               @click="emit('onSelect', value)"
            >
               {{ value }}
            </button>
         </div>
      </div>
   </div>
</template>
