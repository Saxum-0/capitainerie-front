<script setup>
import { reactive } from 'vue';
import { useUserStore } from '@/stores/user';
import api from '@/config/api';
import { useRouter } from 'vue-router';

defineOptions({
  name: 'DashboardPage'
});

const userStore = useUserStore();
const headers = { Authorization: `Bearer ${userStore.token}` };
const router = useRouter();

// ✅ remplacé ref() par reactive()
const userForm = reactive({ id: '', name: '', email: '', password: '' });
const catwayForm = reactive({ id: '', catwayNumber: '', type: '', catwayState: '' });
const reservationForm = reactive({ id: '', catwayNumber: '', clientName: '', boatName: '', checkIn: '', checkOut: '' });

const createUser = async () => {
  await api.post('/users', userForm, { headers });
};

const updateUser = async () => {
  await api.put(`/users/${userForm.id}`, userForm, { headers });
};

const deleteUser = async () => {
  await api.delete(`/users/${userForm.id}`, { headers });
};

const createCatway = async () => {
  await api.post('/catways', catwayForm, { headers });
};

const updateCatway = async () => {
  await api.patch(`/catways/${catwayForm.id}`, { catwayState: catwayForm.catwayState }, { headers });
};

const deleteCatway = async () => {
  await api.delete(`/catways/${catwayForm.id}`, { headers });
};

const getCatwayDetails = () => {
  if (!catwayForm.id) return;
  console.log("🔁 Redirection vers /catway/", catwayForm.id);
  router.push(`/catway/${catwayForm.id}`);
};

const createReservation = async () => {
  await api.post(`/catways/${reservationForm.catwayNumber}/reservations`, reservationForm, { headers });
};

const deleteReservation = async () => {
  await api.delete(`/catways/any/reservations/${reservationForm.id}`, { headers });
};

const getReservationDetails = () => {
  if (!reservationForm.id) return;
  router.push(`/reservation/${reservationForm.id}`);
};
</script>

