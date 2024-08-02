<script setup>
import HeaderComponent from '../components/HeaderComponent.vue'
import SideBarComponent from '../components/SideBarComponent.vue'
</script>

<template>
  <HeaderComponent></HeaderComponent>
  <SideBarComponent></SideBarComponent>

  <main class="w-[70%] pl-[200px] mx-auto pt-10">
    <slot></slot>
  </main>
</template>
