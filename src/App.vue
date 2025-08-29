<template>
  <h1>DevChat</h1>

  <div class="chat-container">
    <div class="messages">
      <div v-for="message in messages" :key="message.id" class="message">
        <strong>{{ message.user }}:</strong>
        <span>{{ message.text }}</span>
      </div>
    </div>

    <form @submit.prevent="sendMessage" class="message-form">
      <input
        v-model="newMessage"
        type="text"
        placeholder="Digite sua mensagem..."
        required
      />
      <button type="submit">Enviar</button>
    </form>
  </div>
</template>

<script>
import { ref, onMounted } from "vue";
import { db } from "../firebase";
import {
  collection,
  addDoc,
  query,
  orderBy,
  onSnapshot,
} from "firebase/firestore";

export default {
  setup() {
    const messages = ref([]);
    const newMessage = ref("");

    const messagesCollection = collection(db, "messages");

    onMounted(() => {
      const q = query(messagesCollection, orderBy("timestamp"));
      onSnapshot(q, (snapshot) => {
        messages.value = snapshot.docs.map((doc) => ({
          id: doc.id,
          ...doc.data(),
        }));
      });
    });

    // Envia uma nova mensagem
    const sendMessage = async () => {
      if (newMessage.value.trim() === "") return;

      await addDoc(messagesCollection, {
        text: newMessage.value,
        user: "Sem usuário",
        timestamp: new Date(),
      });

      newMessage.value = "";
    };

    return {
      messages,
      newMessage,
      sendMessage,
    };
  },
};
</script>

<style>
  h1 {
    text-align: center;
    color: #333;
  }

  .chat-container {
    max-width: 1280px;
    min-width: 80%;
    border: 1px solid #ccc;
    padding: 10px;
    border-radius: 5px;
    background-color: #fff;
  }

  .messages {
    height: 400px;
    overflow-y: auto;
    border-bottom: 1px solid #ccc;
    margin-bottom: 10px;
    padding: 5px;
  }

  .messages .message {
    background-color: #f1f1f1;
    margin-bottom: 5px;
    padding: 10px;
    border-radius: 3px;
    display: flex;
    gap: 10px;
  }

  .messages .message strong {
    font-size: 16px;
    font-weight: bold;
  }

  .messages .message span {
    font-size: 14px;
    word-break: break-word;
    flex: 1;
    white-space: pre-wrap;
    color: #555;
    line-height: 1.4;
    overflow-wrap: break-word;
    hyphens: auto;
    text-align: left;
    display: inline-block;
    vertical-align: top;
    max-width: 100%;
  }

  .message-form {
    display: flex;
    gap: 10px;
  }

  .message-form input {
    flex: 1;
    padding: 10px;
    border: 1px solid #ccc;
    border-radius: 3px;
  }

  .message-form button {
    padding: 10px 20px;
    border: none;
    background-color: #28a745;
    color: white;
    border-radius: 3px;
    cursor: pointer;
  }
</style>
