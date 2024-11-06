<template>
 <div  class="col col-sm-4">
  <div class="card mb-3 p-4">
    <div class="row g-0">
      <div class="col-md-4">
        <div class="skeleton skeleton-avatar"></div>
      </div>
      <div class="col-md-8">
        <div class="card-body">
          <p class="card-title skeleton skeleton-text"></p>
          <p class="card-text skeleton skeleton-text w-75"></p>
          <p class="card-text skeleton skeleton-text w-50"></p>
        </div>
      </div>
    </div>
  </div>
  </div>
</template>

<style scoped>
.skeleton {
  background: #e0e0e0;
  border-radius: 4px;
  animation: pulse 1.5s infinite ease-in-out;
}
.skeleton-avatar {
  width: 100%;
  height: 100px;
  border-radius: 50%;
}
.skeleton-text {
  height: 20px;
  margin-bottom: 10px;
}

/* Animación de pulsado */
@keyframes pulse {
  0% {
    opacity: 1;
  }
  50% {
    opacity: 0.5;
  }
  100% {
    opacity: 1;
  }
}
</style>
