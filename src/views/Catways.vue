<template>
  <div class="catways">
    <h1>Liste des catways</h1>

    <div v-if="loading">Chargement en cours...</div>
    <div v-else-if="error" class="error">{{ error }}</div>
    
    <ul v-else>
      <li v-for="catway in catways" :key="catway._id">
        <strong>ID :</strong> {{ catway._id }} -
        <strong>Numéro :</strong> {{ catway.catwayNumber }} -
        <strong>Type :</strong> {{ catway.type }} -
        <strong>État :</strong> {{ catway.catwayState }}
        <router-link :to="`/catway/${catway._id}`">Détails</router-link>
      </li>
    </ul>
  </div>
</template>

<script setup>
import { onMounted, ref } from 'vue';
import { useUserStore } from '@/stores/user';
import api from '@/config/api';

const catways = ref([]);
const error = ref('');
const loading = ref(true);

const userStore = useUserStore();
const headers = { Authorization: `Bearer ${userStore.token}` };

onMounted(async () => {
  try {
    const res = await api.get('/catways', { headers });
    catways.value = res.data;
  } catch (err) {
    error.value = err.response?.data?.error || 'Erreur lors du chargement.';
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
}
</style>
