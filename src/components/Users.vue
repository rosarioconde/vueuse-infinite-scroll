<template>

  <!-- Mostrar el skeleton si aún no se cargaron los datos iniciales -->
  <UserSkeleton v-if="isLoading" v-for="n in 8" :key="n" />

  <!-- Mostrar los datos cuando estén listos -->
  <div v-for="user in users" :key="user.id" class="col col-sm-4">
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

  <!-- Elemento "sentinel" que dispara la carga de más datos cuando es visible -->
  <div ref="loadMoreTrigger" class="sentinel"></div>
  <UserSkeleton v-if="isLoadingMore" v-for="n in 3" :key="'skeleton-' + n" />

</template>

<script setup>
import axios from 'axios';
import { ref, onMounted } from 'vue';
import { useIntersectionObserver } from '@vueuse/core';
import UserSkeleton from './UserSkeleton.vue';
const API_URL = import.meta.env.VITE_API_URL;
const users = ref([]);
const isLoading = ref(true);
const isLoadingMore = ref(false);
const currentPage = ref(1);
const totalPages = ref(5); // Cambia este valor según el número total de páginas en la API

const loadMore = async () => {
  if (isLoadingMore.value || currentPage.value > totalPages.value) return;

  isLoadingMore.value = true;
  try {
    //TODO: userService
    const response = await axios.get(`${API_URL}/users`, {
      params: {
        page: currentPage.value,
        limit: 10
      }
    });
    users.value.push(...response.data);

    if (response.data.length > 0) {
      currentPage.value++;
    } else {
      totalPages.value = currentPage.value; // Detener el infinite scroll si no hay más datos
    }
  } catch (error) {
    console.error('Error fetching users:', error);
  } finally {
    isLoadingMore.value = false;
    isLoading.value = false;
  }
};

// Observer para detectar el scroll cuando el sentinel entra en el viewport
const loadMoreTrigger = ref(null);
useIntersectionObserver(
  loadMoreTrigger,
  ([{ isIntersecting }]) => {
    if (isIntersecting) loadMore();
  }
);

onMounted(() => {
  loadMore(); // Cargar la primera página de usuarios
});
</script>

<style>
.sentinel {
  height: 1px;
}
</style>
