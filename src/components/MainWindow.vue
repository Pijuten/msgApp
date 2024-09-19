<template>
  <div class="flex w-full h-full mx-auto border rounded-lg overflow-hidden relative">
    <UserList :users="sortedUsers" @load-chat="loadChat" @open-menu="openMenu" />
    <ChatWindow :currentUserId="currentUserId" @open-avatar="openAvatar"
      @updateLastMessageTime="updateLastMessageTime" />

    <MenuOverlay v-if="showMenu" :user="getUserById(selectedUserId)" @close="closeMenu" @open-avatar="openAvatar"
      @view-profile="viewProfile" @send-message="sendMessage" @block-user="blockUser" />

    <AvatarOverlay v-if="showAvatar" :avatar="getUserById(selectedUserId)?.avatar" @close="closeAvatar" />
  </div>
</template>

<script setup>
import { ref, computed } from "vue";
import UserList from "./UserList.vue";
import ChatWindow from "./ChatWindow.vue";
import MenuOverlay from "./MenuOverlay.vue";
import AvatarOverlay from "./AvatarOverlay.vue";

// Mock data
const users = ref([
  { id: 1, name: 'Alice', avatar: '/src/assets/Avatar1.jpg', lastMessageTime: new Date('2024-09-14T10:30:00') },
  { id: 2, name: 'Bob', avatar: '/src/assets/placeholder.svg', lastMessageTime: new Date('2024-09-14T09:45:00') },
]);

const sortedUsers = computed(() => {
  return [...users.value].sort((a, b) => b.lastMessageTime - a.lastMessageTime);
});

const updateLastMessageTime = (userId, newTime) => {
  users.value = users.value.map(user =>
    user.id === userId ? { ...user, lastMessageTime: newTime } : user
  );
};

const getUserById = (userId) => {
  return users.value.find(user => user.id === userId);
}

const currentUserId = ref(0);
const showMenu = ref(false);
const showAvatar = ref(false);
const selectedUserId = ref(null);

const loadChat = (userId) => {
  currentUserId.value = userId;
};

const openMenu = (userId) => {
  selectedUserId.value = userId;
  showMenu.value = true;
};

const openAvatar = (userId) => {
  selectedUserId.value = userId;
  showAvatar.value = true;
};

const closeMenu = () => {
  showMenu.value = false;
};

const closeAvatar = () => {
  showAvatar.value = false;
}

const viewProfile = () => {
  console.log(`View profile of user ${selectedUserId.value}`);
  closeMenu();
};

const sendMessage = () => {
  console.log(`Send message to user ${selectedUserId.value}`);
  closeMenu();
};

const blockUser = () => {
  console.log(`Block user ${selectedUserId.value}`);
  closeMenu();
};

</script>