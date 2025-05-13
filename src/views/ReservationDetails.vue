<template>
  <div class="reservation-details" v-if="reservation.value && reservation.value._id">
    <h1>Détails de la réservation</h1>
    <p><strong>Client :</strong> {{ reservation.value.clientName }}</p>
    <p><strong>Bateau :</strong> {{ reservation.value.boatName }}</p>
    <p><strong>Catway :</strong> {{ reservation.value.catwayNumber }}</p>
    <p><strong>Check-in :</strong> {{ formatDate(reservation.value.checkIn) }}</p>
    <p><strong>Check-out :</strong> {{ formatDate(reservation.value.checkOut) }}</p>
  </div>
  <p v-else>Chargement de la réservation ou introuvable...</p>
</template>

<script setup>
import { ref, onMounted } from 'vue';
import { useRoute } from 'vue-router';
import { useUserStore } from '@/stores/user';
import api from '@/config/api';

const route = useRoute();
const reservation = ref(null);
const userStore = useUserStore();
const headers = { Authorization: `Bearer ${userStore.token}` };

const formatDate = (dateStr) => new Date(dateStr).toLocaleDateString('fr-FR');

onMounted(async () => {
  try {
    const res = await api.get(`/reservations/${route.params.id}`, { headers });
    reservation.value = res.data;
    console.log('✅ Réservation chargée :', reservation.value);
  } catch (err) {
    console.error('❌ Erreur chargement réservation :', err);
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
</style>
