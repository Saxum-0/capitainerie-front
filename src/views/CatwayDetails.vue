<template>
  <div class="catway-details" v-if="catway && catway._id">
    <h1>Détails du catway</h1>
    <p><strong>ID :</strong> {{ catway._id }}</p>
    <p><strong>Numéro :</strong> {{ catway.catwayNumber }}</p>
    <p><strong>Type :</strong> {{ catway.type }}</p>
    <p><strong>État :</strong> {{ catway.catwayState }}</p>
    <p><strong>Créé le :</strong> {{ formatDate(catway.createdAt) }}</p>
    <button @click="goBack">← Retour</button>
  </div>

  <p v-else-if="catway === false">Catway introuvable.</p>
  <p v-else>Chargement du catway...</p>
</template>


<script setup>
import { onMounted, ref } from 'vue';
import { useUserStore } from '@/stores/user';
import api from '@/config/api';

const catways = ref([]);
const loading = ref(true);
const error = ref('');

const userStore = useUserStore();
const headers = { Authorization: `Bearer ${userStore.token}` };

onMounted(async () => {
  try {
    const res = await api.get('/catways', { headers });
    catways.value = Array.isArray(res.data) ? res.data : [];
  } catch (err) {
    console.error('❌ Erreur chargement catways :', err);
    error.value = 'Erreur lors du chargement des catways.';
  } finally {
    loading.value = false;
  }
});
</script>

<style scoped>
.catways {
  max-width: 700px;
  margin: 0 auto;
}
ul {
  list-style: none;
  padding: 0;
}
li {
  margin: 1rem 0;
  padding: 1rem;
  border: 1px solid #ccc;
  border-radius: 5px;
}
.error {
  color: red;
  font-weight: bold;
  margin: 1rem 0;
}
</style>

