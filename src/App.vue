<template>
  <Navbar />
  <Toastest />
  <router-view />
</template>

<script setup>
import Navbar from '@/components/Navbar.vue';
import Toastest from './components/toastest.vue';
import { onMounted } from 'vue';
import { useToast } from 'vue-toastification';

const toast = useToast();

onMounted(() => {
  toast.info('✅ App.vue est bien monté !');
});



</script>

<style>
body {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  margin: 0;
  padding: 0;
  background-color: #f9f9f9;
  color: #333;
}
</style>
