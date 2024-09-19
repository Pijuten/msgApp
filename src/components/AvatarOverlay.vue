<template>
    <div class="fixed inset-0 bg-black bg-opacity-50 flex items-center justify-center z-50" @click="$emit('close')">
      <div class="bg-white p-4 rounded-lg shadow-lg" @click.stop>
        <h2 class="text-lg font-bold mb-2 text-center">User Avatar</h2>
        <div class="flex justify-center mb-4">
          <img class="w-40 h-40 rounded-full" :src="avatar">
        </div>
      </div>
    </div>
  </template>
  
  <script setup>
  defineProps({
    avatar: String,
  });
  defineEmits(['close']);
  </script>