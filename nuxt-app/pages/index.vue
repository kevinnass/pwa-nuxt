<template>
    <div class="flex flex-col items-center">
      <h1 class="text-2xl font-bold">Notifications Push avec Nuxt</h1>
      <button class="rounded-xl mt-6 bg-green-500 h-12 w-96 text-white" @click="askPermission()">Demander l'autorisation de notification</Button>
      <button 
      :class="isNotificationAllowed ? 'rounded-xl mt-6 bg-green-500 h-12 w-96 text-white' : 'rounded-xl mt-6 bg-gray-500 h-12 w-96 text-black cursor-not-allowed'"
      @click="triggerNotification()">Envoyer une notification</button>
    </div>
</template>
  
<script setup lang="ts">
import { ref } from 'vue';
import type { UseWebNotificationOptions } from '@vueuse/core'
import { useWebNotification } from '@vueuse/core'


const isNotificationAllowed = ref(false);

const askPermission = async () => {
    if ('Notification' in window) {
      const permission = await Notification.requestPermission();
      if (permission === 'granted') {
        isNotificationAllowed.value = true;
      }
    } else {
      isNotificationAllowed.value = false;
      alert("Les notifications ne sont pas prises en charge par ce navigateur.")
    }
}
  
const triggerNotification = () => {
    console.log(isNotificationAllowed.value)
    if (isNotificationAllowed.value) {
      new Notification("Nouvelle notification", {
        body: "Ceci est le contenu de la notification",
        icon: '/icon.png' // Mets une icône personnalisée ici si tu le souhaites
      })
    } else {
      // alert("Vous devez autoriser les notifications d'abord.")
    }
}
</script>
  