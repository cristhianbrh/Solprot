<template>
  <main class="h-screen grid grid-rows-[auto_1fr] overflow-none w-screen">
    <HeaderSec/>
    <MainHome/>
  </main>
</template>

<script>
import HeaderSec from './components/HeaderSec.vue'
import MainHome from './components/MainHome.vue'

export default {
  name: 'App',
  components: {
    HeaderSec,
    MainHome
  }
}
</script>

<style>
@tailwind base;
@tailwind components;
@tailwind utilities;


</style>
