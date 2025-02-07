<script setup>
import logo from '../assets/logo.svg'
import { RouterLink, useRoute } from 'vue-router';

const isActiveLink = (routePath) => {
  const route = useRoute();
  return route.path === routePath
}
</script>

<template>
    <nav class="bg-green-700 border-b w-full border-green-500">
      <div class="mx-auto max-w-7xl px-2 sm:px-6 lg:px-8">
        <div class="flex h-20 items-center justify-between">
          <div
            class="flex flex-1 items-center justify-center md:items-stretch md:justify-start"
          >
            <!-- Logo -->
            <RouterLink class="flex flex-shrink-0 items-center mr-4" to="/">
              <img class="h-10 w-auto" :src="logo" alt="Vue Logo" />
              <span class="hidden md:block text-white text-2xl font-bold ml-2"
                >Vue Task Evaluation</span
              >
            </RouterLink>
            <div class="md:ml-auto">
              <div class="flex space-x-2">
                <RouterLink
                  to="/"
                  :class="[ isActiveLink('/') ? 'bg-green-900' : 'hover:bg-gray-900', 'text-white hover:bg-green-900 hover:text-white rounded-md px-3 py-2']"
                  >Home</RouterLink
                >
                <RouterLink
                  to="/registration"
                  :class="[ isActiveLink('/registration') ? 'bg-green-900' : 'hover:bg-gray-900', 'text-white hover:bg-green-900 hover:text-white rounded-md px-3 py-2']"
                  >Registeration</RouterLink
                >
                <RouterLink
                  to="/description"
                  :class="[ isActiveLink('/description') ? 'bg-green-900' : 'hover:bg-gray-900', 'text-white hover:bg-green-900 hover:text-white rounded-md px-3 py-2']"
                  >Description</RouterLink
                >
              </div>
            </div>
          </div>
        </div>
      </div>
    </nav>
</template>