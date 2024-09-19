<template>
    <div class="fixed inset-0 bg-black bg-opacity-50 flex items-center justify-center z-50" @click="$emit('close')">
      <div class="bg-white p-4 rounded-lg shadow-lg" @click.stop>
        <h2 class="text-lg font-bold mb-2 text-center">User Menu</h2>
        <ul>
          <li class="flex justify-center mb-4">
            <img class="w-16 h-16 rounded-full" :src="user.avatar" :alt="user.name" @click.stop="$emit('open-avatar', user.id)">
          </li>
          <li class="text-sm font-medium text-center mb-2">{{ user.name }}</li>
          <li class="cursor-pointer hover:bg-gray-100 p-2" @click="$emit('view-profile')">View Profile</li>
          <li class="cursor-pointer hover:bg-gray-100 p-2" @click="$emit('send-message')">Send Message</li>
          <li class="cursor-pointer hover:bg-gray-100 p-2" @click="$emit('block-user')">Block User</li>
        </ul>
      </div>
    </div>
  </template>
  
  <script setup>
  defineProps({
    user: Object,
  });
  defineEmits(['close', 'open-avatar', 'view-profile', 'send-message', 'block-user']);
  </script>