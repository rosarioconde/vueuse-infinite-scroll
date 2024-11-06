<template>
  <UserSkeleton v-if="isLoading" v-for="n in 8" :key="n" />
  <transition-group name="fade" v-if="!isLoading">
    <div v-for="user in users" :key="user.id" class="col col-sm-4 fade-item">
      <div class="card mb-3 p-4">
        <div class="row g-0">
          <div class="col-md-4">
            <img :src="user.avatar" class="img-fluid rounded-start" alt="avatar" />
          </div>
          <div class="col-md-8">
            <div class="card-body">
              <h5 class="card-title">{{ user.name }}</h5>
              <p class="card-text">{{ user.email }}</p>
            </div>
          </div>
        </div>
      </div>
    </div>
  </transition-group>

</template>

<script setup>
import axios from 'axios';
import { ref, onMounted } from 'vue';
import UserSkeleton from './UserSkeleton.vue';
const API_URL = import.meta.env.VITE_API_URL;


const users = ref([]);
const isLoading = ref(true);

const fetchUsers = async () => {
  try {
    const response = await axios.get(`${API_URL}/users`);
    users.value = response.data;
  } catch (error) {
    console.error('Error fetching users:', error);
  } finally {
    isLoading.value = false;
  }
};

onMounted(fetchUsers);


</script>

<style>

.fade-enter-active,
.fade-leave-active {
  transition: opacity 0.5s ease;

}

.fade-enter-from,
.fade-leave-to {
  opacity: 0;
}

.fade-item {
  opacity: 1;
}
</style>
