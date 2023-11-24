<script setup>
import { onMounted, ref } from 'vue'
let message = ref('AYE')
let messages = ref(['hello', 'world']);
let socket = null;
onMounted(() => {
    socket = new WebSocket('ws://firstwebsocket-backend.onrender.com/primus');

    //listen for messages from websockets server
    socket.onmessage = (event) => {
        let newMessage = JSON.parse(event.data);
        if(newMessage.action === "newMessage"){
            messages.value.push(newMessage.message);
        }

    };
})
const sendMessage = () => {
    let newMessage = {
        "message": message.value,
        "action": "newMessage"
    }
    socket.send(JSON.stringify(newMessage));
}
</script>

<template>
    <div>
        <ul>
            <li v-for="m in messages">{{ m }}</li>
        </ul>
        <div>
            <input v-model="message" type="text">
            <button @click="sendMessage">Send</button>
        </div>
    </div>
</template>

<style scoped></style>
