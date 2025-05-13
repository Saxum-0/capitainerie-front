<!-- src/components/ToastTest.vue -->
<template>
  <div>
    <button @click="testToast">Test Toast</button>
  </div>
</template>

<script setup>
import { useToast } from 'vue-toastification';

const toast = useToast();

const testToast = () => {
  toast.success("✅ Ça fonctionne !");
};
</script>
