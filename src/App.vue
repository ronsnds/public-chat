<template>
  <h1>Chat em Tempo Real</h1>

  <div>
    <div>
      <div v-for="message in messages" :key="message.id" >
        <strong>{{ message.user }}:</strong> {{ message.text }}
      </div>
    </div>

    <form @submit.prevent="sendMessage">
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
  name: "Chat",
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

</style>
