<script setup>
import { RouterView } from 'vue-router'
</script>

<template>
  <header class="mb-4">
    <div class="d-flex justify-content-center">
      <h1>Моя Vue страница</h1>
    </div>
    <div class="d-flex w-100">
      <div class="logo">
        <a href="https://gsr-rabota.ru/" target="_blank">
          <img src="https://gsr-rabota.ru/_nuxt/img/newlogo.e6ba86e.svg" alt="logo" />
        </a>
      </div>
      <nav>
        <div class="w-100 h-100 align-items-center d-flex justify-content-around">
          <template v-for="n in 4" :key="n">
            <div>
              <a href="/" class="mx-2">Ссылка #{{ n }}</a>
            </div>
          </template>
        </div>

      </nav>
    </div>
  </header>

  <RouterView />
</template>

<style scoped>
.logo {
  width: 130px;
}

nav {
  width: 100%;
}
</style>
