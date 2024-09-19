<template>
  <div class="min-h-screen w-full flex flex-col">
    <!-- Main content area -->
    <main class="flex-grow overflow-auto">
      <div v-if="currentTab === 'chat'" class="h-full w-full flex items-start justify-center">
        <MainWindow />
      </div>
      <div v-else-if="currentTab === 'search'" class="h-full flex items-center justify-center">
        <h1 class="text-3xl font-bold text-gray-800">Search</h1>
      </div>
      <div v-else-if="currentTab === 'notifications'" class="h-full flex items-center justify-center">
        <h1 class="text-3xl font-bold text-gray-800">Notifications</h1>
      </div>
      <div v-else-if="currentTab === 'profile'" class="h-full flex items-center justify-center">
        <h1 class="text-3xl font-bold text-gray-800">Profile</h1>
      </div>
    </main>

    <!-- Bottom tabs -->
    <nav class="bg-white shadow-lg sticky bottom-0 w-full">
      <ul class="flex justify-around">
        <li v-for="tab in tabs" :key="tab.name" class="flex-1">
          <button 
            @click="currentTab = tab.name"
            class="w-full py-4 px-2 flex flex-col items-center justify-center transition-colors duration-200"
            :class="currentTab === tab.name ? 'text-blue-500' : 'text-gray-600 hover:text-blue-500'"
          >
            <component :is="tab.icon" class="h-6 w-6" />
            <span class="mt-1 text-xs">{{ tab.label }}</span>
          </button>
        </li>
      </ul>
    </nav>
  </div>
</template>

<script setup>
import MainWindow from './MainWindow.vue';
import { ref } from 'vue'
import { HomeIcon, SearchIcon, BellIcon, UserIcon } from 'lucide-vue-next'

const tabs = [
  { name: 'chat', label: 'Chat', icon: HomeIcon },
  { name: 'group', label: 'Group', icon: SearchIcon },
  { name: 'contacts', label: 'contacts', icon: BellIcon },
  { name: 'profile', label: 'Profile', icon: UserIcon },
]

const currentTab = ref('chat')
</script>

<style scoped>
/* Add any component-specific styles here */
</style>