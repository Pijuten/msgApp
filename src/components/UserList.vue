<template>
    <div class="w-64 border-r h-full flex flex-col overflow-hidden">
      <div class="flex-grow overflow-y-auto">
        <div class="p-4 space-y-4">
          <UserItem 
            v-for="user in users" 
            :key="user.id" 
            :user="user" 
            @load-chat="$emit('load-chat', user.id)"
            @open-menu="$emit('open-menu', user.id)"
          />
        </div>
      </div>
    </div>
  </template>
  
  <script setup>
  import UserItem from './UserItem.vue';
  
  defineProps({
    users: Array,
  });
  defineEmits(['load-chat', 'open-menu']);
  </script>