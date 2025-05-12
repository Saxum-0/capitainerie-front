<template>
  <div class="reservation-details">
    <h1>Détails de la réservation</h1>

    <div v-if="loading">Chargement en cours...</div>
    <div v-else-if="error" class="error">{{ error }}</div>
    <div v-else-if="reservation">
      <p><strong>ID :</strong> {{ reservation._id }}</p>
      <p><strong>Client :</strong> {{ reservation.clientName }}</p>
      <p><strong>Bateau :</strong> {{ reservation.boatName }}</p>
      <p><strong>Catway :</strong> {{ reservation.catwayNumber }}</p>
      <p><strong>Check-in :</strong> {{ reservation.checkIn }}</p>
      <p><strong>Check-out :</strong> {{ reservation.checkOut }}</p>
    </div>
  </div>
</template>

<script setup>
import { ref, onMounted } from 'vue';
import { useRoute } from 'vue-router';
import { useUserStore } from '@/stores/user';
import api from '@/config/api';

const route = useRoute();
const reservation = ref(null);
const error = ref('');
const loading = ref(true);

const userStore = useUserStore();
const headers = { Authorization: `Bearer ${userStore.token}` };

onMounted(async () => {
  try {
    const res = await api.get(`/reservations/${route.params.id}`, { headers });
    reservation.value = res.data;
  } catch (err) {
    error.value = err.response?.data?.error || 'Erreur lors du chargement de la réservation.';
  } finally {
    loading.value = false;
  }
});
</script>

<style scoped>
.reservation-details {
  max-width: 600px;
  margin: 2rem auto;
  padding: 1rem;
  border: 1px solid #ccc;
  border-radius: 5px;
}
.error {
  color: red;
  font-weight: bold;
}
</style>

  