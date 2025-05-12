<template>
  <div class="catway-details">
    <h1>Détails du catway</h1>

    <div v-if="loading">Chargement en cours...</div>
    <div v-else-if="error" class="error">{{ error }}</div>
    <div v-else-if="catway">
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

const route = useRoute();
const catway = ref(null);
const error = ref('');
const loading = ref(true);

const userStore = useUserStore();
const headers = { Authorization: `Bearer ${userStore.token}` };

onMounted(async () => {
  try {
    const res = await api.get(`/catways/${route.params.id}`, { headers });
    catway.value = res.data;
  } catch (err) {
    error.value = err.response?.data?.error || 'Erreur lors du chargement du catway.';
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
.error {
  color: red;
  font-weight: bold;
}
</style>
