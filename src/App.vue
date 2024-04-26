<template>
  <main>
    <HelloWorld msg="Welcome to Your Vue.js App"/>
    <HeaderSec/>
    <MainHome/>
  </main>
</template>

<script>
import HelloWorld from './components/HelloWorld.vue'
import HeaderSec from './components/HeaderSec.vue'
import MainHome from './components/MainHome.vue'

export default {
  name: 'App',
  components: {
    HelloWorld,
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
