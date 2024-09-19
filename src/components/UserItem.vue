<template>
    <div class="flex items-center space-x-4 pb-4 border-b cursor-pointer" @click="$emit('load-chat')">
      <div class="w-8 h-8 rounded-full overflow-hidden">
        <img :src="user.avatar" :alt="user.name" class="w-full h-full object-cover" @click.stop="$emit('open-menu')" />
      </div>
      <span class="text-sm font-medium">{{ user.name }}</span>
    </div>
  </template>
  
  <script setup>
  defineProps({
    user: Object,
  });
  defineEmits(['load-chat', 'open-menu']);
  </script>