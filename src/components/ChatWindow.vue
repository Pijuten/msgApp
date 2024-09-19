<template>
    <!-- Chat window -->
    <div class="flex-1 flex flex-col h-full overflow-hidden">
        <div class="flex items-center p-2 bg-gray-100 border-b">
            <div class="w-8 h-8 rounded-full bg-blue-500 flex items-center justify-center text-white font-bold text-lg mr-3"
                @click.stop="$emit('open-avatar', currentUserId)">

            </div>
            <span class="text-lg font-semibold text-gray-800">{{ getUserName(currentUserId) }}</span>
        </div>
        <div ref="messagesContainer" class="flex-1 overflow-y-auto p-4">
            <div class="space-y-4">
                <div v-for="message in messages" :key="message.id" class="space-y-1">
                    <div class="flex items-baseline justify-between">
                        <span class="text-sm font-medium text-gray-900">{{ getUserName(message.userId) }}</span>
                        <span class="text-xs text-gray-500">{{ message.timestamp }}</span>
                    </div>
                    <p class="text-sm text-gray-800 bg-gray-100 rounded-lg p-2">{{ message.text }}</p>
                </div>
            </div>
        </div>
        <div class="p-4 border-t">
            <form @submit.prevent="sendMessage" class="flex space-x-2">
                <input ref="messageInput" v-model="newMessage"
                    class="flex-1 px-3 py-2 border rounded-md focus:outline-none focus:ring-2 focus:ring-blue-500"
                    placeholder="Type a message..." />
                <button type="submit"
                    class="px-4 py-2 bg-blue-500 text-white rounded-md hover:bg-blue-600 focus:outline-none focus:ring-2 focus:ring-blue-500 focus:ring-offset-2">
                    <SendIcon class="h-4 w-4" />
                    <span class="sr-only">Send</span>
                </button>
            </form>
        </div>
    </div>
</template>


<script setup>
import { ref, onMounted, nextTick, defineProps, watch } from 'vue'
import { SendIcon } from 'lucide-vue-next'

const props = defineProps({
    currentUserId: {
        type: Number,
        required: true
    }
})

const messages = ref([]);

// Function to load chat messages
const loadChats = (currentUserId) => {
    switch (currentUserId) {
        case 2:
            return ([
                { id: 1, userId: 0, text: "Hey everyone!", timestamp: "10:00 AM" },
                { id: 2, userId: 2, text: "Hi Alice!", timestamp: "10:01 AM" },
                { id: 3, userId: 0, text: "How's it going?", timestamp: "10:02 AM" },
                { id: 4, userId: 2, text: "Great! How about you?", timestamp: "10:03 AM" },
                { id: 5, userId: 0, text: "Just joined. What did I miss?", timestamp: "10:05 AM" },
            ])
        case 1:
            return ([
                { id: 1, userId: 0, text: "Hey everyone!", timestamp: "10:00 AM" },
                { id: 2, userId: 1, text: "Hi Alice!", timestamp: "10:01 AM" },
            ])
        default:
            return []
    }
}

// Watch for changes in currentUserId and load the appropriate chat
watch(() => props.currentUserId, (newUserId) => {
    messages.value = loadChats(newUserId);
    nextTick(() => scrollToBottom());
}, { immediate: true })


const newMessage = ref('');

// Function to get the username by userId
const getUserName = (userId) => {
    switch (userId) {
        case 0:
            return "You"
        case 2:
            return "Bob"
        case 1:
            return "Alice"
        default:
            return "Unknown User"
    }
}

// Function to send a new message
const sendMessage = () => {
    if (newMessage.value.trim()) {
        messages.value.push({
            id: messages.value.length + 1,
            userId: props.currentUserId || 1, // Use the provided userId or default to 1
            text: newMessage.value,
            timestamp: new Date().toLocaleTimeString([], { hour: '2-digit', minute: '2-digit' })
        })
        newMessage.value = ''
        emit('updateLastMessageTime', props.currentUserId, new Date())
        nextTick(() => scrollToBottom())
    }
}

// Handle the scroll to bottom behavior
const messagesContainer = ref(null)

const scrollToBottom = () => {
    if (messagesContainer.value) {
        messagesContainer.value.scrollTop = messagesContainer.value.scrollHeight
    }
}

onMounted(() => {
    nextTick(() => {
        scrollToBottom()
        this.$refs.messageInput.focus()

    })
})
const emit = defineEmits(['open-avatar', 'updateLastMessageTime'])
</script>