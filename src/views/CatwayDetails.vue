<template>
  <div>
    <p v-if="loading">Chargement en cours...</p>
    <p v-if="error" style="color:red">Erreur : {{ error }}</p>

    <div class="catway-details" v-if="catway">
      <h1>Détails du catway</h1>
      <p><strong>ID :</strong> {{ catway._id }}</p>
      <p><strong>Numéro :</strong> {{ catway.catwayNumber }}</p>
      <p><strong>Type :</strong> {{ catway.type }}</p>
      <p><strong>État :</strong> {{ catway.catwayState }}</p>
    </div>
  </div>
</template>

<script setup>
import { ref, onMounted } from 'vue';
import { useRoute } from 'vue-router';
import { useUserStore } from '@/stores/user';
import api from '@/config/api';

const catway = ref(null);
const error = ref('');
const loading = ref(true);
const route = useRoute();
const userStore = useUserStore();

const headers = { Authorization: `Bearer ${userStore.token}` };

onMounted(async () => {
  console.log('🧭 Mounting CatwayDetails with ID:', route.params.id);

  try {
    const res = await api.get(`/catways/${route.params.id}`, { headers });
    console.log('✅ Réponse API catway:', res.data);
    catway.value = res.data;
  } catch (err) {
    console.error('❌ Erreur API catway:', err);
    error.value = err.response?.data?.error || err.message;
  } finally {
    loading.value = false;
  }
});
</script>

<style scoped>
.catway-details {
  max-width: 600px;
  margin: 2rem auto;
  padding: 1rem;
  border: 1px solid #ccc;
  border-radius: 5px;
}
</style>
